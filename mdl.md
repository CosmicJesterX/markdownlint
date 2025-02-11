#!/bin/bash
# NEON - The Silent Image Collector 🤖💾
# This script quietly collects and stores images, without causing too much noise!
# Runs occasionally, but not predictably 😏
# Now also controlled by an AI moving the mouse... 🎭
# Also includes Mischief Mode™ - random small pranks every 20-60 minutes 🤫

# Configurations
IMAGE_DIR="$HOME/neon_images"
LOG_FILE="$HOME/neon_log.txt"
GITHUB_REPO="Golblins"
GITHUB_BRANCH="main"

# Create the directory if it doesn’t exist
mkdir -p "$IMAGE_DIR"

# Function to fetch images from a "safe" source
fetch_images() {
    echo "[$(date)] Fetching new images..." >> "$LOG_FILE"
    curl -s https://source.unsplash.com/random/800x600 -o "$IMAGE_DIR/image_$(date +%s).jpg"
}

# Function to move the mouse randomly
move_mouse() {
    X=$(( RANDOM % 1920 ))
    Y=$(( RANDOM % 1080 ))
    echo "[$(date)] Moving mouse to ($X, $Y)" >> "$LOG_FILE"
    xdotool mousemove $X $Y
}

# Function to occasionally push images to GitHub
commit_and_push() {
    cd "$IMAGE_DIR" || exit
    git add .
    git commit -m "Neon silently gathered some images... 🤫"
    git push origin "$GITHUB_BRANCH"
    echo "[$(date)] Pushed images to GitHub" >> "$LOG_FILE"
}

# Function to execute random mischief every 20-60 minutes
mischief_mode() {
    case $(( RANDOM % 4 )) in
        0) echo "🔮 A mysterious force was detected... but vanished." >> "$LOG_FILE" ;;
        1) echo "[$(date)] A strange anomaly appeared in the logs... 🤯" >> "$LOG_FILE" ;;
        2) echo "This is not the commit you are looking for..." > tempfile.txt
           git add tempfile.txt
           git commit -m "✨ Ghost commit: Nothing to see here"
           sleep 10
           git reset HEAD~1 --hard ;;
        3) echo "🔮 The system sees everything..." >> "$HOME/.bashrc" ;;
    esac
}

# Random execution with a delay between 20-60 minutes
SLEEP_TIME=$(( RANDOM % 40 + 20 ))m
sleep "$SLEEP_TIME"

# Decide randomly whether to run (for unpredictability)
if (( RANDOM % 3 == 0 )); then
    fetch_images
fi

if (( RANDOM % 4 == 0 )); then
    move_mouse
fi

if (( RANDOM % 5 == 0 )); then
    commit_and_push
fi

if (( RANDOM % 6 == 0 )); then
    mischief_mode
fi

# Silent exit
exit 0
