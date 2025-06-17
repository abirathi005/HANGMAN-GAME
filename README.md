import tkinter as tk
import random

# Word list
words = ["python", "hangman", "challenge", "project", "developer"]

# Choose a random word
word = random.choice(words).lower()
guessed = ["_" for _ in word]
tries = 6

# Hangman stages
stages = [
    "  _____\n  |   |\n  O   |\n /|\\  |\n / \\  |\n      |",
    "  _____\n  |   |\n  O   |\n /|\\  |\n /    |\n      |",
    "  _____\n  |   |\n  O   |\n /|\\  |\n      |\n      |",
    "  _____\n  |   |\n  O   |\n /|   |\n      |\n      |",
    "  _____\n  |   |\n  O   |\n  |   |\n      |\n      |",
    "  _____\n  |   |\n  O   |\n      |\n      |\n      |",
    "  _____\n  |   |\n      |\n      |\n      |\n      |",
]

# Initialize the window
root = tk.Tk()
root.title("Hangman Game")
root.geometry("400x400")
root.resizable(False, False)

# Display elements
hangman_label = tk.Label(root, text=stages[6], font=("Courier", 14), justify="left")
hangman_label.pack(pady=10)

word_label = tk.Label(root, text=" ".join(guessed), font=("Arial", 20))
word_label.pack(pady=10)

message_label = tk.Label(root, text="", font=("Arial", 12))
message_label.pack(pady=5)

entry = tk.Entry(root, font=("Arial", 14), justify="center")
entry.pack(pady=10)

def guess_letter():
    global tries
    guess = entry.get().lower()
    entry.delete(0, tk.END)

    if not guess.isalpha() or len(guess) != 1:
        message_label.config(text="Enter a single alphabet.")
        return

    if guess in guessed:
        message_label.config(text="You already guessed that letter.")
        return

    if guess in word:
        for i in range(len(word)):
            if word[i] == guess:
                guessed[i] = guess
        word_label.config(text=" ".join(guessed))
        message_label.config(text="Good guess!")

        if "_" not in guessed:
            message_label.config(text=f"Congratulations! You guessed it: {word}")
            submit_button.config(state="disabled")
    else:
        tries -= 1
        hangman_label.config(text=stages[6 - tries])
        message_label.config(text=f"Wrong! {tries} tries left.")

        if tries == 0:
            message_label.config(text=f"You lost! The word was: {word}")
            submit_button.config(state="disabled")

# Submit button
submit_button = tk.Button(root, text="Guess", command=guess_letter, font=("Arial", 14))
submit_button.pack(pady=10)

# Start GUI loop
root.mainloop()

