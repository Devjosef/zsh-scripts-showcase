# Fun and Lesser-Known Zsh Scripts

Description
A collection of scripts and automations that you can run within the zsh shell!

Script

!/bin/zsh

Directory containing wallpapers

WALLPAPER_DIR="$HOME/Pictures/Wallpapers"

Get a random wallpaper

RANDOM_WALLPAPER=$(ls $WALLPAPER_DIR | sort -R | tail -1)

Set the wallpaper using feh (change to your preferred tool)

feh --bg-scale "$WALLPAPER_DIR/$RANDOM_WALLPAPER"

chmod +x random_wallpaper.sh

./random_wallpaper.sh

DEPENDENCIES
 `feh` (or preferred wallpaper setting tool)


# DIRECTORY BOOKMARKS

Description
Create, list, and jump to directory bookmarks, making navigation across 
frequently used directories seamless.

Script

!/bin/zsh

Bookmarks file
BOOKMARKS_FILE="$HOME/.dir_bookmarks"

Function to add a bookmark
bookmark_add() {
    echo "$1=$PWD" >> $BOOKMARKS_FILE
    echo "Bookmark added: $1 -> $PWD"
}

Function to list bookmarks
bookmark_list() {
    cat $BOOKMARKS_FILE
}

Function to jump to a bookmark
bookmark_jump() {
    local DEST=$(grep "^$1=" $BOOKMARKS_FILE | cut -d '=' -f 2-)
    if [ -n "$DEST" ]; then
        cd "$DEST"
    else
        echo "Bookmark not found: $1"
    fi
}

Alias for ease of use
alias badd=bookmark_add
alias blist=bookmark_list
alias bjump=bookmark_jump

Usage
Add a bookmark: badd bookmark_name
List bookmarks: blist
Jump to a bookmark: bjump bookmark_name

### WEATHER INFO FETCHER

Description
Fetch and display current weather information for a specified city using a 
command-line weather API.

Script

!/bin/zsh

Function to get weather
get_weather() {
    local CITY="$1"
    local API_KEY="your_api_key_here"
    local RESPONSE=$(curl -s 
"http://api.openweathermap.org/data/2.5/weather?q=$CITY&appid=$API_KEY&units=metric")

    local TEMP=$(echo $RESPONSE | jq '.main.temp')
    local DESC=$(echo $RESPONSE | jq -r '.weather[0].description')

    echo "Current weather in $CITY: $TEMP°C, $DESC"
}

Usage
Replace your_api_key_here with your actual API key from OpenWeatherMap.
Run the script with a city name:
sh
get_weather "London"

# Dependencies
`curl`
`jq`

### GIT BRANCH CLEANER

Description
List and delete local branches that have been merged into the current 
branch to keep your git repository clean.

Script
!/bin/zsh

Function to delete merged branches
clean_merged_branches() {
    git branch --merged | grep -v '^\*' | xargs -n 1 git branch -d
}

Usage
Run the script to delete merged branches:
sh
clean_merged_branches

### TIMER

Description
A simple timer script to count down from a specified number of seconds. 
Great for time management and focused work sessions. can be used as a 
pomodoro timer!

Script
!/bin/zsh

Function to start a timer
timer() {
    local SECONDS=$1
    while [ $SECONDS -gt 0 ]; do
        echo -ne "$SECONDS\033[0K\r"
        sleep 1
        : $((SECONDS--))
    done
    echo "Time's up!"
}

Usage
Run the script with the number of seconds to count down:
sh
timer 10

### Command History Search

Description
Quickly search your command history for a specific term, making it easier 
to find and reuse past commands.

Script
!/bin/zsh
 
Function to search command history
history_search() {
    history | grep "$1"
}

Usage
Run the script with the term you want to search for:
sh
history_search "git"

### Temporary HTTP Server

Description
Quickly start a temporary HTTP server to serve files from a directory. 
Useful for sharing files or testing locally.

Script

!/bin/zsh

Function to start a temporary HTTP server
start_http_server() {
    local PORT=${1:-8000}
    echo "Starting HTTP server on port $PORT..."
    python -m http.server $PORT
}

Usage
Run the script with the desired port number:
sh
start_http_server 8000

# Dependencies
`python`

### Automated Backup Script

Description

Automate the backup of important files or directories to a specified 
location.

Script
!/bin/zsh

Source and destination directories
SOURCE_DIR="$HOME/Documents"
DEST_DIR="$HOME/Backup"

Create destination directory if it doesn't exist
mkdir -p "$DEST_DIR"

Rsync to backup files
rsync -av --delete "$SOURCE_DIR/" "$DEST_DIR/"

echo "Backup completed at $(date)"

Usage
Save script as `backup.sh`, and make it executable with `chmod +x 
backup.sh` run it to perform the backup.

### Automated System Update

Description
Automatically update your system packages.

Script
!/bin/zsh

Update package lists
sudo apt update

Upgrade installed packages
sudo apt upgrade -y

echo "System update completed at $(date)"

Usage
Save the script as `update_system.sh` make it executable by running `chmod 
+x update_system.sh` and run it to update your system.

### Automated Email Notification

Description
Send an email notification after a long-running task is completed.

Script
!/bin/zsh

Your email address
EMAIL="your-email@example.com"

Long-running task (e.g., data processing)
echo "Starting long-running task..."
sleep 60  # Simulate a long-running task with sleep

Send email notification
echo "The long-running task has completed." | mail -s "Task Completed" 
"$EMAIL"

echo "Notification sent to $EMAIL"

Usage
Save the script as `notify.sh` make it executable `chmod +x notify.sh` and 
run it to perform the task and send the notification.

### Automated File Organizer

Description
Automatically organize files in a directory based on their type.

Script
!/bin/zsh

Source directory
SOURCE_DIR="$HOME/Downloads"

File type directories
IMAGE_DIR="$SOURCE_DIR/Images"
DOCUMENT_DIR="$SOURCE_DIR/Documents"
VIDEO_DIR="$SOURCE_DIR/Videos"
OTHER_DIR="$SOURCE_DIR/Others"

Create directories if they don't exist
mkdir -p "$IMAGE_DIR" "$DOCUMENT_DIR" "$VIDEO_DIR" "$OTHER_DIR"

Move files to corresponding directories
for FILE in "$SOURCE_DIR"/*; do
    if [[ -f "$FILE" ]]; then
        case "$(file --mime-type -b "$FILE")" in
            image/*) mv "$FILE" "$IMAGE_DIR" ;;
            text/*|application/pdf) mv "$FILE" "$DOCUMENT_DIR" ;;
            video/*) mv "$FILE" "$VIDEO_DIR" ;;
            *) mv "$FILE" "$OTHER_DIR" ;;
        esac
    fi
done

echo "Files organized at $(date)"

Usage
Save this script as `organize_files.sh` make it executable `chmod +x 
organize_files.sh` and run it to organize files in your Downloads 
directory.

### Automated Git Commit and Push

Description
Automatically commit and push changes to a Git repository.

Script
!/bin/zsh

Change to the repository directory
REPO_DIR="$HOME/projects/my-repo"
cd "$REPO_DIR"

Add all changes
git add .

Commit changes with a message
COMMIT_MESSAGE="Automated commit at $(date)"
git commit -m "$COMMIT_MESSAGE"

Push changes to the remote repository
git push origin main

echo "Changes committed and pushed at $(date)"

Usage
Save this script as `auto_commit_push.sh` make it executable ` chmod +x 
auto_committ_push.sh` and run it to automatically commit and push changes 
to your repository.

### Running Automations on Schedule

Description
You can use cron to schedule these scripts to run at specific intervals.

Script

Edit the crontab:
sh
crontab -e

Add a cron job to run the script
sh
Run backup script daily at 2am
0 2 * * * /path/to/backup.sh

Run system update script weekly on Sundays at 3am
0 3 * * 0 /path/to/update_system.sh

Run file organizer script hourly
0 * * * * /path/to/organize_files.sh



These scripts showcase the versatility and power of Zsh for various fun 
and practical tasks. Feel free to try them out and modify them to suit 
your needs. Happy scripting!
