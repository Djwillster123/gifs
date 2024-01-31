import streamlit as st
import subprocess
import os

def split_video(input_file, output_prefix, segment_duration):
    try:
        # Get video duration
        duration_cmd = f'ffprobe -i {input_file} -show_entries format=duration -v quiet -of csv="p=0"'
        duration = float(subprocess.check_output(duration_cmd, shell=True, stderr=subprocess.STDOUT).decode("utf-8").strip())

        # Calculate number of segments
        num_segments = int(duration // segment_duration)

        # Split the video
        for i in range(num_segments):
            start_time = i * segment_duration
            output_file = f'{output_prefix}_{i + 1}.mp4'
            split_cmd = f'ffmpeg -i {input_file} -ss {start_time} -t {segment_duration} -c copy {output_file}'
            subprocess.run(split_cmd, shell=True, stderr=subprocess.STDOUT, stdout=subprocess.PIPE)

            # Update output display
            st.text(f'Video segment {i + 1} successfully created: {output_file}')

        st.text(f'Video successfully split into {num_segments} segments.')
        return num_segments

    except subprocess.CalledProcessError as e:
        st.error(f'Error: {e.output.decode("utf-8")}')
        return 0

def main():
    st.title("Video Splitter App")
    input_video = st.file_uploader("Upload Video File", type=["mp4"])
    output_folder = st.text_input("Output Folder Path")

    if st.button("Split Video"):
        if input_video is not None and output_folder:
            output_prefix = 'output_segment'
            segment_duration = 5  # seconds

            num_segments = split_video(input_video.name, output_prefix, segment_duration)

            if num_segments > 0:
                st.success(f'Video successfully split into {num_segments} segments.')

if __name__ == "__main__":
    main()
