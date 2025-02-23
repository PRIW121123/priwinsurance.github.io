<!DOCTYPE html>
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
        <input type="text" id="general-contractor" placeholder="General Contractor"><br><br>
        <input type="text" id="job-address" placeholder="Job Address"><br><br>
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
    </div>
    
<script>
        const users = {};
        let jobs = [];

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
            const jobName = document.getElementById("job-name").value.trim();
            const generalContractor = document.getElementById("general-contractor").value.trim();
            const jobAddress = document.getElementById("job-address").value.trim();
            
            if (jobName && generalContractor && jobAddress) {
                jobs.push({ name: jobName, generalContractor, jobAddress, subcontractors: [] });
                document.getElementById("job-status").textContent = "Job created successfully!";
                renderJobList();
                document.getElementById("job-name").value = "";
                document.getElementById("general-contractor").value = "";
                document.getElementById("job-address").value = "";
            } else {
                document.getElementById("job-status").textContent = "Please fill out all fields.";
            }
        }
        
        function renderJobList() {
            const jobList = document.getElementById("job-list");
            jobList.innerHTML = "";
            jobs.forEach((job, index) => {
                const li = document.createElement("li");
                li.textContent = `${job.name} - ${job.generalContractor}, ${job.jobAddress}`;
                li.onclick = () => openJobDashboard(index);
                jobList.appendChild(li);
            });
        }
        
        function openJobDashboard(index) {
            document.getElementById("admin-container").classList.add("hidden");
            document.getElementById("job-dashboard").classList.remove("hidden");
            document.getElementById("job-title").textContent = jobs[index].name;
            renderSubcontractorSelect();
        }
        
        function renderSubcontractorSelect() {
            const subSelect = document.getElementById("subcontractor-select");
            subSelect.innerHTML = "";
            Object.keys(users).forEach(email => {
                if (users[email].role === "subcontractor") {
                    const option = document.createElement("option");
                    option.value = email;
                    option.textContent = users[email].company;
                    subSelect.appendChild(option);
                }
            });
        }
    </script>
</body>
</html>
