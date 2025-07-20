import os
import zipfile
from datetime import datetime

def backup_to_zip(folder_path):
    """
    Compresses the specified folder into a ZIP archive.
    
    Parameters:
    folder_path (str): The path of the folder to backup.
    """
    # Check if the folder exists
    if not os.path.exists(folder_path):
        print("❌ Folder not found! Please check the path.")
        return

    # Create a unique backup filename with current date and time
    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
    backup_filename = f"backup_{timestamp}.zip"
    backup_path = os.path.join(os.getcwd(), backup_filename)

    # Create a ZIP file and write contents
    with zipfile.ZipFile(backup_path, 'w', zipfile.ZIP_DEFLATED) as backup_zip:
        # Walk through the folder
        for foldername, subfolders, filenames in os.walk(folder_path):
            for filename in filenames:
                file_path = os.path.join(foldername, filename)
                # Calculate relative path to keep folder structure
                rel_path = os.path.relpath(file_path, folder_path)
                backup_zip.write(file_path, rel_path)

    print(f"✅ Backup completed successfully!\nSaved as: {backup_path}")

# ======================
# Main Execution Section
# ======================
if __name__ == "__main__":
    # Change the path below to the folder you want to back up
    folder_to_backup = input("Enter the full path of the folder to back up: ").strip()

    # Run the backup function
    backup_to_zip(folder_to_backup)
