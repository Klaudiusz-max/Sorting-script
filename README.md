# Sorting-script

import os
import shutil
import send2trash
import daemon
import time

desktop = '/home/klaudiusz/Desktop'

mapping = {
       '.pdf': '/home/klaudiusz/Desktop/pdf',
    '.txt': '/home/klaudiusz/Desktop/txt',
    '.jpg': '/home/klaudiusz/Desktop/jpg',
    '.py': '/home/klaudiusz/Desktop/projekty',
    '.pdf': '/home/klaudiusz/Desktop/pdf',
    '.txt': '/home/klaudiusz/Desktop/txt',
    '.jpg': '/home/klaudiusz/Desktop/jpg',
    '.png': '/home/klaudiusz/Desktop/png',
    '.docx': '/home/klaudiusz/Desktop/documents',
    '.xlsx': '/home/klaudiusz/Desktop/documents',
    '.pptx': '/home/klaudiusz/Desktop/documents',
    '.mp3': '/home/klaudiusz/Desktop/music',
    '.mp4': '/home/klaudiusz/Desktop/videos',
    '.zip': '/home/klaudiusz/Desktop/archives',
    '.exe': '/home/klaudiusz/Desktop/programs',
    '.deb': '/home/klaudiusz/Desktop/programs',
    '.sh': '/home/klaudiusz/Desktop/scripts',
    '.html': '/home/klaudiusz/Desktop/websites',
    '.css': '/home/klaudiusz/Desktop/websites',
    '.js': '/home/klaudiusz/Desktop/scripts',
    '.json': '/home/klaudiusz/Desktop/data',
    '.xml': '/home/klaudiusz/Desktop/data',
    '.csv': '/home/klaudiusz/Desktop/data',
    '.svg': '/home/klaudiusz/Desktop/jpg',
    '.gif': '/home/klaudiusz/Desktop/jpg',
    '.bmp': '/home/klaudiusz/Desktop/jpg',
    '.tiff': '/home/klaudiusz/Desktop/jpg',
    '.avi': '/home/klaudiusz/Desktop/videos',
    '.mov': '/home/klaudiusz/Desktop/videos',
    '.wav': '/home/klaudiusz/Desktop/audio',
    '.aac': '/home/klaudiusz/Desktop/audio',
    '.html': '/home/klaudiusz/Desktop/websites',
    '.css': '/home/klaudiusz/Desktop/websites',
    '.xlsx': '/home/klaudiusz/Desktop/spreadsheets',
    '.pptx': '/home/klaudiusz/Desktop/presentations',
    '.odt': '/home/klaudiusz/Desktop/documents',
    '.ods': '/home/klaudiusz/Desktop/documents',
    '.odp': '/home/klaudiusz/Desktop/documents',
    '.java': '/home/klaudiusz/Desktop/java',
    '.class': '/home/klaudiusz/Desktop/java',
    '.jsp': '/home/klaudiusz/Desktop/java',
    '.war': '/home/klaudiusz/Desktop/java',
    '.ear': '/home/klaudiusz/Desktop/java',
    '.c': '/home/klaudiusz/Desktop/c_cpp',
    '.cpp': '/home/klaudiusz/Desktop/c_cpp',
    '.h': '/home/klaudiusz/Desktop/c_cpp',
    '.hpp': '/home/klaudiusz/Desktop/c_cpp',
    '.cs': '/home/klaudiusz/Desktop/c_sharp',
    '.vb': '/home/klaudiusz/Desktop/visual_basic',
}

# Create directories based on the mapping
for directory in mapping.values():
    if not directory.endswith("trash") and not os.path.exists(directory):
        os.makedirs(directory)

def sorting():
    # Move files based on their extensions
    for filename in os.listdir(desktop):
        source_file_path = os.path.join(desktop, filename)
        if os.path.isfile(source_file_path):
            file_extension = os.path.splitext(filename)[1]
            if file_extension in mapping:
                destination_directory = mapping[file_extension]
                if destination_directory.endswith("trash"):
                    send2trash.send2trash(source_file_path)
                    print(f"File {filename} has been moved to the trash.")
                else:
                    shutil.move(source_file_path, os.path.join(destination_directory, filename))
                    print(f"File {filename} has been moved to {destination_directory}/")

    # Remove empty directories
    for root, directories, files in os.walk(desktop, topdown=False):
        for directory in directories:
            dir_path = os.path.join(root, directory)
            if not os.listdir(dir_path):
                os.rmdir(dir_path)
                print(f"Folder {dir_path} has been deleted.")

# Daemon function
def run_as_daemon():
    with daemon.DaemonContext():
        while True:
            sorting()
            time.sleep(60)

if __name__ == '__main__':
    run_as_daemon()
