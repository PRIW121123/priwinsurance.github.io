PRIW - Job Compliance
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Sign Up</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        .hidden { display: none; }
    </style>
</head>
<body>
    <div id="auth-container">
        <h2>Login or Sign Up</h2>
        <input type="email" id="email" placeholder="Email"><br><br>
        <input type="password" id="password" placeholder="Password"><br><br>
        <input type="text" id="company" placeholder="Company Name (only for sign up)"><br><br>
        <button onclick="authenticate()">Login</button>
        <button onclick="register()">Sign Up</button>
        <p id="auth-error" style="color:red;"></p>
    </div>
    
<div id="upload-container" class="hidden">
        <h2>Upload Insurance Document</h2>
        <p id="company-name"></p>
        <input type="file" id="fileUpload"><br><br>
        <div id="policy-fields">
            <div class="policy-entry">
                <input type="text" class="insurer" placeholder="Insurer Name"><br><br>
                <input type="text" class="policy-number" placeholder="Policy Number"><br><br>
                <label>Policy Effective Date:</label><br>
                <input type="date" class="effective-date"><br><br>
                <label>Policy Expiration Date:</label><br>
                <input type="date" class="expiration-date"><br><br>
            </div>
        </div>
        <button onclick="addPolicyFields()">Add Another Policy</button><br><br>
        <button onclick="uploadFile()">Upload</button>
        <p id="upload-status"></p>
    </div>

<script>
        const users = {};
        const uploadedFiles = [];

        function register() {
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            const company = document.getElementById("company").value;

            if (email && password && company) {
                if (users[email]) {
                    document.getElementById("auth-error").textContent = "Email already registered!";
                } else {
                    users[email] = { password: password, company: company };
                    authenticate();
                }
            } else {
                document.getElementById("auth-error").textContent = "Please fill out all fields for sign-up.";
            }
        }

        function authenticate() {
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            
            if (users[email] && users[email].password === password) {
                document.getElementById("auth-container").classList.add("hidden");
                document.getElementById("upload-container").classList.remove("hidden");
                document.getElementById("company-name").textContent = "Company: " + users[email].company;
            } else {
                document.getElementById("auth-error").textContent = "Invalid email or password!";
            }
        }
        
        function addPolicyFields() {
            const policyFields = document.getElementById("policy-fields");
            const newEntry = document.createElement("div");
            newEntry.classList.add("policy-entry");
            newEntry.innerHTML = `
                <input type="text" class="insurer" placeholder="Insurer Name"><br><br>
                <input type="text" class="policy-number" placeholder="Policy Number"><br><br>
                <label>Policy Effective Date:</label><br>
                <input type="date" class="effective-date"><br><br>
                <label>Policy Expiration Date:</label><br>
                <input type="date" class="expiration-date"><br><br>
            `;
            policyFields.appendChild(newEntry);
        }
        
        function uploadFile() {
            const fileInput = document.getElementById("fileUpload");
            const policyEntries = document.querySelectorAll(".policy-entry");
            
            if (fileInput.files.length > 0 && policyEntries.length > 0) {
                let policies = [];
                policyEntries.forEach(entry => {
                    const insurer = entry.querySelector(".insurer").value;
                    const policyNumber = entry.querySelector(".policy-number").value;
                    const effectiveDate = entry.querySelector(".effective-date").value;
                    const expirationDate = entry.querySelector(".expiration-date").value;
                    
                    if (insurer && policyNumber && effectiveDate && expirationDate) {
                        policies.push({ insurer, policyNumber, effectiveDate, expirationDate });
                    }
                });
                
                if (policies.length > 0) {
                    uploadedFiles.push({
                        file: fileInput.files[0].name,
                        policies: policies
                    });
                    document.getElementById("upload-status").textContent = "File uploaded successfully with policies!";
                } else {
                    document.getElementById("upload-status").textContent = "Please fill in all policy fields.";
                }
            } else {
                document.getElementById("upload-status").textContent = "Please select a file and add at least one policy.";
            }
        }
    </script>
</body>
</html>
