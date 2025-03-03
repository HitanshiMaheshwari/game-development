# game-development
#index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guess the Number Game</title>
</head>
<body>
    <h1>Welcome to Guess the Number Game!</h1>
    <p>Guess a number between 1 and 100:</p>
    
    <form method="POST">
        <input type="number" name="guess" min="1" max="100" required>
        <button type="submit">Guess</button>
    </form>

    <p>{{ message }}</p>
</body>
</html>



# app.py
from flask import Flask, render_template, request, redirect, url_for
import random

app = Flask(__name__)

# Initialize the random number for guessing
secret_number = random.randint(1, 100)
attempts = 0

@app.route("/", methods=["GET", "POST"])
def home():
    global secret_number, attempts
    message = ""
    
    if request.method == "POST":
        try:
            guess = int(request.form["guess"])
            attempts += 1

            if guess < secret_number:
                message = "Too low! Try again."
            elif guess > secret_number:
                message = "Too high! Try again."
            else:
                message = f"Congratulations! You guessed the number in {attempts} attempts."
                # Reset the game after success
                secret_number = random.randint(1, 100)
                attempts = 0
        except ValueError:
            message = "Please enter a valid number."

    return render_template("index.html", message=message)

if __name__ == "__main__":
    app.run(debug=True)
