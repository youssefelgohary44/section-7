from flask import Flask, request
import subprocess

app = Flask(__name__)

# Vulnerability: hardcoded secret
app.secret_key = "my_super_secret_key_123"


@app.route("/")
def home():
    return """
    <h1>Vulnerable Flask Application</h1>
    <p>Test the following endpoints:</p>
    <ul>
        <li>/command?cmd=whoami</li>
        <li>/calculate?expression=1%2B2</li>
    </ul>
    """


# Vulnerability: OS command injection through shell=True
@app.route("/command")
def command():
    cmd = request.args.get("cmd", "")

    result = subprocess.check_output(
        cmd,
        shell=True,
        text=True
    )

    return f"<pre>{result}</pre>"


# Vulnerability: unsafe evaluation of user input
@app.route("/calculate")
def calculate():
    expression = request.args.get("expression", "")

    result = eval(expression)

    return f"<h2>Result: {result}</h2>"


if __name__ == "__main__":
    # Vulnerability: debug mode enabled and listening on all interfaces
    app.run(host="0.0.0.0", port=5000, debug=True)
