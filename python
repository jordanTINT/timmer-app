import tkinter as tk
import time
import threading
import winsound
from datetime import datetime

class TimerApp:
    def __init__(self, root):
        self.root = root
        self.root.configure(bg="black")
        self.seconds = 0
        self.running = False
        self.paused = False
        self.countdown_mode = tk.BooleanVar(value=False)
        self.sound_on = tk.BooleanVar(value=True)

        self.label = tk.Label(
            root,
            text="Days: 00  Hours: 00  Minutes: 00  Seconds: 00",
            font=("Arial", 20),
            bg="black",
            fg="green"
        )
        self.label.pack(padx=20, pady=20)

        # Mode selection
        self.mode_frame = tk.Frame(root, bg="black")
        self.mode_frame.pack()
        self.stopwatch_radio = tk.Radiobutton(
            self.mode_frame, text="Stopwatch", variable=self.countdown_mode, value=False,
            bg="black", fg="green", selectcolor="black", activebackground="black", activeforeground="green"
        )
        self.stopwatch_radio.pack(side=tk.LEFT)
        self.countdown_radio = tk.Radiobutton(
            self.mode_frame, text="Countdown", variable=self.countdown_mode, value=True,
            bg="black", fg="green", selectcolor="black", activebackground="black", activeforeground="green"
        )
        self.countdown_radio.pack(side=tk.LEFT)

        # Time entry for countdown
        entry_frame = tk.Frame(root, bg="black")
        entry_frame.pack(pady=5)
        self.day_entry = tk.Entry(entry_frame, width=3, fg="green", bg="black", insertbackground="green")
        self.hour_entry = tk.Entry(entry_frame, width=3, fg="green", bg="black", insertbackground="green")
        self.min_entry = tk.Entry(entry_frame, width=3, fg="green", bg="black", insertbackground="green")
        self.sec_entry = tk.Entry(entry_frame, width=3, fg="green", bg="black", insertbackground="green")
        tk.Label(entry_frame, text="Days", fg="green", bg="black").pack(side=tk.LEFT)
        self.day_entry.pack(side=tk.LEFT)
        tk.Label(entry_frame, text="Hours", fg="green", bg="black").pack(side=tk.LEFT)
        self.hour_entry.pack(side=tk.LEFT)
        tk.Label(entry_frame, text="Minutes", fg="green", bg="black").pack(side=tk.LEFT)
        self.min_entry.pack(side=tk.LEFT)
        tk.Label(entry_frame, text="Seconds", fg="green", bg="black").pack(side=tk.LEFT)
        self.sec_entry.pack(side=tk.LEFT)
        set_btn = tk.Button(entry_frame, text="Set Time", command=self.set_time, bg="black", fg="green", activebackground="black", activeforeground="green")
        set_btn.pack(side=tk.LEFT, padx=10)

        # Sound on/off checkbox
        self.sound_check = tk.Checkbutton(
            root, text="Sound On", variable=self.sound_on,
            bg="black", fg="green", selectcolor="black", activebackground="black", activeforeground="green"
        )
        self.sound_check.pack()

        # Frame to center the buttons
        self.button_frame = tk.Frame(root, bg="black")
        self.button_frame.pack(pady=10)

        self.start_btn = tk.Button(
            self.button_frame,
            text="Start",
            command=self.start,
            bg="black",
            fg="green",
            activebackground="black",
            activeforeground="green"
        )
        self.start_btn.pack(side=tk.LEFT, padx=10)

        self.pause_btn = tk.Button(
            self.button_frame,
            text="Pause",
            command=self.pause_resume,
            bg="black",
            fg="green",
            activebackground="black",
            activeforeground="green"
        )
        self.pause_btn.pack(side=tk.LEFT, padx=10)

        self.reset_btn = tk.Button(
            self.button_frame,
            text="Reset",
            command=self.reset,
            bg="black",
            fg="green",
            activebackground="black",
            activeforeground="green"
        )
        self.reset_btn.pack(side=tk.LEFT, padx=10)

        # Current date/time label
        self.datetime_label = tk.Label(root, text="", font=("Arial", 12), bg="black", fg="green")
        self.datetime_label.pack(pady=5)
        self.update_datetime()

    def update_datetime(self):
        now = datetime.now().strftime("Date: %Y-%m-%d   Time: %H:%M:%S")
        self.datetime_label.config(text=now)
        self.root.after(1000, self.update_datetime)

    def set_time(self):
        try:
            days = int(self.day_entry.get() or 0)
            hours = int(self.hour_entry.get() or 0)
            mins = int(self.min_entry.get() or 0)
            secs = int(self.sec_entry.get() or 0)
            self.seconds = days * 86400 + hours * 3600 + mins * 60 + secs
            self.label.config(text=f"Days: {days:02d}  Hours: {hours:02d}  Minutes: {mins:02d}  Seconds: {secs:02d}")
        except ValueError:
            self.label.config(text="Invalid input!")

    def update_timer(self):
        while self.running:
            if not self.paused:
                days, rem = divmod(self.seconds, 86400)
                hours, rem = divmod(rem, 3600)
                mins, secs = divmod(rem, 60)
                timer = f"Days: {days:02d}  Hours: {hours:02d}  Minutes: {mins:02d}  Seconds: {secs:02d}"
                self.label.config(text=timer)
                if self.countdown_mode.get():
                    # Beep every second in countdown mode
                    if self.sound_on.get() and self.seconds > 0:
                        winsound.Beep(800, 100)
                    if self.seconds == 0:
                        self.running = False
                        self.label.config(text="Time's up!")
                        # Triple beep when time is up
                        if self.sound_on.get():
                            for _ in range(3):
                                winsound.Beep(1000, 300)
                                time.sleep(0.2)
                        break
                    self.seconds -= 1
                else:
                    # Stopwatch mode beeps (hour/half-hour)
                    if self.sound_on.get():
                        if mins == 0 and secs == 0 and self.seconds != 0:
                            winsound.Beep(1000, 300)
                            time.sleep(0.2)
                            winsound.Beep(1000, 300)
                        elif mins == 30 and secs == 0:
                            winsound.Beep(1000, 500)
                    self.seconds += 1
                time.sleep(1)
            else:
                time.sleep(0.1)

    def start(self):
        if not self.running:
            self.running = True
            self.paused = False
            self.pause_btn.config(text="Pause")
            threading.Thread(target=self.update_timer, daemon=True).start()

    def pause_resume(self):
        if self.running:
            self.paused = not self.paused
            self.pause_btn.config(text="Resume" if self.paused else "Pause")

    def reset(self):
        self.running = False
        self.paused = False
        if self.countdown_mode.get():
            self.set_time()
        else:
            self.seconds = 0
            self.label.config(text="Days: 00  Hours: 00  Minutes: 00  Seconds: 00")
        self.pause_btn.config(text="Pause")

if __name__ == "__main__":
    root = tk.Tk()
    root.title("Study Stopwatch & Countdown")
    app = TimerApp(root)
    root.mainloop()
