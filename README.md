<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Upload</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        .hidden { display: none; }
    </style>
</head>
<body>
    <div id="signup-container">
        <h2>Sign Up</h2>
        <input type="email" id="new-email" placeholder="Email"><br><br>
        <input type="password" id="new-password" placeholder="Choose Password"><br><br>
        <input type="text" id="company" placeholder="Company Name"><br><br>
        <button onclick="register()">Sign Up</button>
        <p id="signup-error" style="color:red;"></p>
    </div>

    <div id="login-container" class="hidden">
        <h2>Login</h2>
        <input type="email" id="email" placeholder="Email"><br><br>
        <input type="password" id="password" placeholder="Password"><br><br>
        <button onclick="authenticate()">Login</button>
        <p id="error" style="color:red;"></p>
    </div>
    
    <div id="upload-container" class="hidden">
        <h2>Upload Insurance Document</h2>
        <p id="company-name"></p>
        <input type="file" id="fileUpload"><br><br>
        <button onclick="uploadFile()">Upload</button>
        <p id="upload-status"></p>
    </div>

    <script>
        const users = {};

        function register() {
            const newEmail = document.getElementById("new-email").value;
            const newPass = document.getElementById("new-password").value;
            const company = document.getElementById("company").value;

            if (newEmail && newPass && company) {
                if (users[newEmail]) {
                    document.getElementById("signup-error").textContent = "Email already registered!";
                } else {
                    users[newEmail] = { password: newPass, company: company };
                    document.getElementById("signup-container").classList.add("hidden");
                    document.getElementById("login-container").classList.remove("hidden");
                }
            } else {
                document.getElementById("signup-error").textContent = "Please fill out all fields.";
            }
        }

        function authenticate() {
            const email = document.getElementById("email").value;
            const pass = document.getElementById("password").value;
            
            if (users[email] && users[email].password === pass) {
                document.getElementById("login-container").classList.add("hidden");
                document.getElementById("upload-container").classList.remove("hidden");
                document.getElementById("company-name").textContent = "Company: " + users[email].company;
            } else {
                document.getElementById("error").textContent = "Invalid email or password!";
            }
        }
        
        function uploadFile() {
            const fileInput = document.getElementById("fileUpload");
            if (fileInput.files.length > 0) {
                document.getElementById("upload-status").textContent = "File uploaded successfully!";
            } else {
                document.getElementById("upload-status").textContent = "Please select a file.";
            }
        }
    </script>
</body>
</html>
