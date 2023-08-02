#shell script .
1)  Write a shell script to automate the process of creating and managing backups for a
    directory.


#!/bin/bash

# Configuration
backup_source="/path/to/source_directory"
backup_destination="/path/to/backup_destination"
backup_prefix="backup_$(date +%Y%m%d%H%M%S)"

# Create backup
backup_filename="${backup_prefix}.tar.gz"
backup_path="${backup_destination}/${backup_filename}"

echo "Creating backup..."
tar -czf "$backup_path" "$backup_source"

if [ $? -eq 0 ]; then
echo "Backup created: $backup_path"
else
echo "Backup creation failed!"
exit 1
fi

# Manage backups (optional)
max_backups=5  # Maximum number of backups to keep

# List existing backups, sort by modification time, and delete excess backups
existing_backups=($(ls -t "${backup_destination}"/backup_*.tar.gz))
num_backups=${#existing_backups[@]}

if [ $num_backups -gt $max_backups ]; then
backups_to_delete=$((num_backups - max_backups))
for ((i = 0; i < $backups_to_delete; i++)); do
echo "Deleting old backup: ${existing_backups[$i]}"
rm "${existing_backups[$i]}"
done
fi

echo "Backup process completed."


2) Write a script to automate the deployment of a web application, including steps such as
   copying files, setting up configuration files, and starting the application.
   #!/bin/bash

# Variables
app_dir="/path/to/your/web/app"       # Directory of your web application
config_dir="/path/to/config/files"    # Directory containing configuration files
app_user="your_app_user"              # User to run the application
app_group="your_app_group"            # Group for the application
app_command="start_command_here"      # Command to start your application
app_port=80                           # Port your application runs on

# Step 1: Backup previous deployment (optional)
backup_dir="/path/to/backup/directory"
backup_timestamp=$(date +%Y%m%d%H%M%S)
backup_name="backup_${backup_timestamp}"
tar -czf "${backup_dir}/${backup_name}.tar.gz" "$app_dir" "$config_dir"

# Step 2: Copy new files
echo "Copying new files..."
rsync -av --delete "$app_dir/" "$config_dir/" /path/to/deployment/directory/

# Step 3: Set up configuration files
echo "Setting up configuration files..."
# You may need to copy or symlink configuration files to appropriate locations

# Step 4: Start the application
echo "Starting the application..."
cd "$app_dir"
su -s /bin/bash -c "$app_command" "$app_user"

# Step 5: Verify application startup (optional)
sleep 5  # Wait for a few seconds for the application to start
if ss -tln | grep -q ":$app_port"; then
echo "Application started successfully."
else
echo "Application did not start as expected."
fi

echo "Deployment process completed."
