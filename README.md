PRIW Subcontractor Compliance
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Portal</title>
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
        <select id="role">
            <option value="subcontractor">Subcontractor</option>
            <option value="admin">Administrator</option>
        </select><br><br>
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
    
<div id="admin-container" class="hidden">
        <h2>Administrator Dashboard</h2>
        <h3>Create New Job</h3>
        <input type="text" id="job-name" placeholder="Job Name"><br><br>
        <button onclick="createJob()">Create Job</button>
        <p id="job-status"></p>
        <h3>Existing Jobs</h3>
        <ul id="job-list"></ul>
    </div>
    
<div id="job-dashboard" class="hidden">
        <h2 id="job-title"></h2>
        <h3>Invite Subcontractors</h3>
        <select id="subcontractor-select"></select>
        <button onclick="inviteSubcontractor()">Invite</button>
        <p id="invite-status"></p>
        <h3>Assigned Subcontractors</h3>
        <ul id="subcontractor-list"></ul>
        <h3>Upload Forms for Subcontractors</h3>
        <div id="form-uploads">
            <input type="file" class="formUpload"><br><br>
        </div>
        <button onclick="addFormUploadField()">Add Another Form</button><br><br>
        <button onclick="uploadForm()">Upload Forms</button>
        <p id="form-status"></p>
        <h3>Available Forms</h3>
        <ul id="form-list"></ul>
    </div>
    
<script>
        const users = {};
        const jobs = [];

        function register() {
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            const company = document.getElementById("company").value;
            const role = document.getElementById("role").value;

            if (email && password && company) {
                if (users[email]) {
                    document.getElementById("auth-error").textContent = "Email already registered!";
                } else {
                    users[email] = { password, company, role, jobs: [], documents: [], assignedForms: [] };
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
                if (users[email].role === "admin") {
                    document.getElementById("admin-container").classList.remove("hidden");
                    renderJobList();
                }
            } else {
                document.getElementById("auth-error").textContent = "Invalid email or password!";
            }
        }
        
        function createJob() {
            const jobName = document.getElementById("job-name").value;
            if (jobName) {
                jobs.push({ name: jobName, subcontractors: [] });
                document.getElementById("job-status").textContent = "Job created successfully!";
                renderJobList();
            }
        }
        
        function addFormUploadField() {
            const formUploads = document.getElementById("form-uploads");
            const newInput = document.createElement("input");
            newInput.type = "file";
            newInput.className = "formUpload";
            formUploads.appendChild(document.createElement("br"));
            formUploads.appendChild(newInput);
            formUploads.appendChild(document.createElement("br"));
        }
        
        function uploadForm() {
            const formInputs = document.querySelectorAll(".formUpload");
            formInputs.forEach(input => {
                if (input.files.length > 0) {
                    console.log("Uploaded form:", input.files[0].name);
                }
            });
            document.getElementById("form-status").textContent = "Forms uploaded successfully!";
        }
    </script>
</body>
</html>

