Heres a sneak peak of v1.0-beta.1 ( STEAM SUPPORTED ONLY, DOESN'T FULLY WORK )
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
pip install opencv-python numpy pyautogui mss ( in pwrshell )
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
import cv2
import numpy as np
import pyautogui
import mss
import time
def start_bot():
    print("Bot starting in 3 seconds. Tab into Geometry Dash!")
    time.sleep(3)
    # Define the "Vision Box" (Region of Interest)
    # This is a box just IN FRONT of your player icon. 
    # You WILL need to tweak these numbers based on your monitor resolution and GD window size.
    vision_box = {"top": 500, "left": 600, "width": 100, "height": 80}
    print("Running... Press 'q' in the vision window to stop.")
    # mss is used for ultra-fast screen capturing
    with mss.mss() as sct:
        while True:
            # 1. Grab the pixels inside the vision box
            screenshot = sct.grab(vision_box)
            img = np.array(screenshot)
            # 2. Convert to grayscale to make obstacle detection easier
            gray = cv2.cvtColor(img, cv2.COLOR_BGRA2GRAY)
            # 3. Basic Obstacle Detection Logic
            # We are checking if any pixels in the box are dark enough to be an obstacle
            # Spikes and blocks usually have distinct outlines.
            threshold_value = 60 # Adjust this depending on the level's background color
            
            # If the bot sees dark pixels (less than the threshold), it assumes an obstacle
            if np.any(gray < threshold_value):
                print("Obstacle detected! JUMP!")
                pyautogui.click() 
                
                # Add a tiny cooldown so it doesn't spam click 1000 times a second
                time.sleep(0.15)
            # 4. Display what the bot sees (Helpful for debugging)
            # You can comment this out later to make the bot run faster
            cv2.imshow("What the Bot Sees", gray)
            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
    cv2.destroyAllWindows()
if name == "__main__":
    start_bot()
