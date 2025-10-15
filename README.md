# User-authentication-system
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Secure User Authentication System</title>
<style>
  :root {
    --primary-color: #3498db;
    --primary-dark: #2980b9;
    --secondary-color: #2ecc71;
    --secondary-dark: #27ae60;
    --danger-color: #e74c3c;
    --danger-dark: #c0392b;
    --text-color: #333;
    --light-gray: #f5f5f5;
    --border-color: #ddd;
    --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1em;
  }
  
  .container {
    width: 100%;
    max-width: 450px;
    background-color: white;
    border-radius: 12px;
    box-shadow: var(--shadow);
    overflow: hidden;
    transition: all 0.3s ease;
  }
  
  .header {
    background: linear-gradient(to right, var(--primary-color), var(--primary-dark));
    color: white;
    padding: 1.5em;
    text-align: center;
  }
  
  .header h1 {
    font-size: 1.8rem;
    margin-bottom: 0.5em;
  }
  
  .header p {
    opacity: 0.9;
  }
  
  .form-container {
    padding: 2em;
  }
  
  .form-group {
    margin-bottom: 1.5em;
    position: relative;
  }
  
  .form-group label {
    display: block;
    margin-bottom: 0.5em;
    font-weight: 500;
    color: var(--text-color);
  }
  
  .form-control {
    width: 100%;
    padding: 0.75em 1em;
    border: 1px solid var(--border-color);
    border-radius: 6px;
    font-size: 1rem;
    transition: border-color 0.3s;
  }
  
  .form-control:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
  }
  
  .password-strength {
    margin-top: 0.5em;
    height: 4px;
    border-radius: 2px;
    background-color: var(--light-gray);
    overflow: hidden;
  }
  
  .password-strength-meter {
    height: 100%;
    width: 0;
    transition: width 0.3s, background-color 0.3s;
  }
  
  .password-strength.weak .password-strength-meter {
    width: 25%;
    background-color: var(--danger-color);
  }
  
  .password-strength.medium .password-strength-meter {
    width: 50%;
    background-color: #f39c12;
  }
  
  .password-strength.strong .password-strength-meter {
    width: 100%;
    background-color: var(--secondary-color);
  }
  
  .password-requirements {
    margin-top: 0.5em;
    font-size: 0.85rem;
    color: #666;
  }
  
  .requirement {
    display: flex;
    align-items: center;
    margin-bottom: 0.25em;
  }
  
  .requirement i {
    margin-right: 0.5em;
    font-size: 0.75rem;
  }
  
  .requirement.valid {
    color: var(--secondary-color);
  }
  
  .requirement.invalid {
    color: #999;
  }
  
  .btn {
    display: block;
    width: 100%;
    padding: 0.75em;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .btn-primary {
    background-color: var(--primary-color);
    color: white;
  }
  
  .btn-primary:hover {
    background-color: var(--primary-dark);
  }
  
  .btn-secondary {
    background-color: var(--secondary-color);
    color: white;
  }
  
  .btn-secondary:hover {
    background-color: var(--secondary-dark);
  }
  
  .btn-danger {
    background-color: var(--danger-color);
    color: white;
  }
  
  .btn-danger:hover {
    background-color: var(--danger-dark);
  }
  
  .toggle-link {
    display: block;
    text-align: center;
    margin-top: 1.5em;
    color: var(--primary-color);
    cursor: pointer;
    text-decoration: underline;
  }
  
  .message {
    padding: 0.75em;
    border-radius: 6px;
    margin-top: 1em;
    text-align: center;
    display: none;
  }
  
  .message.success {
    background-color: rgba(46, 204, 113, 0.2);
    color: var(--secondary-dark);
    display: block;
  }
  
  .message.error {
    background-color: rgba(231, 76, 60, 0.2);
    color: var(--danger-dark);
    display: block;
  }
  
  .dashboard {
    padding: 2em;
    text-align: center;
  }
  
  .dashboard h2 {
    margin-bottom: 1em;
    color: var(--text-color);
  }
  
  .user-info {
    background-color: var(--light-gray);
    padding: 1.5em;
    border-radius: 8px;
    margin-bottom: 1.5em;
  }
  
  .user-info p {
    margin-bottom: 0.5em;
  }
  
  .action-buttons {
    display: flex;
    gap: 1em;
  }
  
  .action-buttons .btn {
    flex: 1;
  }
  
  .hidden {
    display: none;
  }
  
  .loading {
    position: relative;
    pointer-events: none;
    opacity: 0.7;
  }
  
  .loading::after {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    width: 20px;
    height: 20px;
    margin: -10px 0 0 -10px;
    border: 2px solid transparent;
    border-top: 2px solid white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  
  @media (max-width: 480px) {
    .container {
      max-width: 100%;
    }
    
    .form-container {
      padding: 1.5em;
    }
    
    .action-buttons {
      flex-direction: column;
    }
  }
</style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>Secure Authentication</h1>
      <p>Register or login to access your account</p>
    </div>
    
    <!-- Authentication Form -->
    <div id="auth-form" class="form-container">
      <h2 id="form-title">Create Account</h2>
      
      <form id="auth-form-element">
        <div class="form-group">
          <label for="username">Username</label>
          <input type="text" id="username" class="form-control" placeholder="Enter your username" autocomplete="off" />
        </div>
        
        <div class="form-group">
          <label for="email">Email Address</label>
          <input type="email" id="email" class="form-control" placeholder="Enter your email" autocomplete="off" />
        </div>
        
        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" id="password" class="form-control" placeholder="Enter your password" autocomplete="off" />
          
          <div class="password-strength">
            <div class="password-strength-meter"></div>
          </div>
          
          <div class="password-requirements">
            <div class="requirement invalid" id="req-length">
              <i class="fas fa-circle"></i> At least 8 characters
            </div>
            <div class="requirement invalid" id="req-uppercase">
              <i class="fas fa-circle"></i> At least one uppercase letter
            </div>
            <div class="requirement invalid" id="req-lowercase">
              <i class="fas fa-circle"></i> At least one lowercase letter
            </div>
            <div class="requirement invalid" id="req-number">
              <i class="fas fa-circle"></i> At least one number
            </div>
            <div class="requirement invalid" id="req-special">
              <i class="fas fa-circle"></i> At least one special character
            </div>
          </div>
        </div>
        
        <div class="form-group">
          <label for="confirm-password">Confirm Password</label>
          <input type="password" id="confirm-password" class="form-control" placeholder="Confirm your password" autocomplete="off" />
        </div>
        
        <button type="submit" id="submit-btn" class="btn btn-primary">Create Account</button>
      </form>
      
      <span id="toggle-link" class="toggle-link">Already have an account? Login here</span>
      
      <div id="message" class="message"></div>
    </div>
    
    <!-- Dashboard -->
    <div id="dashboard" class="dashboard hidden">
      <h2>Welcome, <span id="user-display"></span>!</h2>
      
      <div class="user-info">
        <p><strong>Username:</strong> <span id="dashboard-username"></span></p>
        <p><strong>Email:</strong> <span id="dashboard-email"></span></p>
        <p><strong>Account Created:</strong> <span id="account-created"></span></p>
      </div>
      
      <div class="action-buttons">
        <button id="change-password-btn" class="btn btn-secondary">Change Password</button>
        <button id="logout-btn" class="btn btn-danger">Logout</button>
      </div>
    </div>
    
    <!-- Change Password Form -->
    <div id="change-password-form" class="form-container hidden">
      <h2>Change Password</h2>
      
      <form id="password-change-form">
        <div class="form-group">
          <label for="current-password">Current Password</label>
          <input type="password" id="current-password" class="form-control" placeholder="Enter current password" />
        </div>
        
        <div class="form-group">
          <label for="new-password">New Password</label>
          <input type="password" id="new-password" class="form-control" placeholder="Enter new password" />
        </div>
        
        <div class="form-group">
          <label for="confirm-new-password">Confirm New Password</label>
          <input type="password" id="confirm-new-password" class="form-control" placeholder="Confirm new password" />
        </div>
        
        <div class="action-buttons">
          <button type="submit" class="btn btn-primary">Update Password</button>
          <button type="button" id="cancel-change-password" class="btn btn-secondary">Cancel</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    (function(){
      // DOM Elements
      const authFormElement = document.getElementById('auth-form-element');
      const usernameInput = document.getElementById('username');
      const emailInput = document.getElementById('email');
      const passwordInput = document.getElementById('password');
      const confirmPasswordInput = document.getElementById('confirm-password');
      const submitBtn = document.getElementById('submit-btn');
      const toggleLink = document.getElementById('toggle-link');
      const formTitle = document.getElementById('form-title');
      const message = document.getElementById('message');
      const authForm = document.getElementById('auth-form');
      const dashboard = document.getElementById('dashboard');
      const userDisplay = document.getElementById('user-display');
      const dashboardUsername = document.getElementById('dashboard-username');
      const dashboardEmail = document.getElementById('dashboard-email');
      const accountCreated = document.getElementById('account-created');
      const logoutBtn = document.getElementById('logout-btn');
      const changePasswordBtn = document.getElementById('change-password-btn');
      const changePasswordForm = document.getElementById('change-password-form');
      const passwordChangeForm = document.getElementById('password-change-form');
      const cancelChangePassword = document.getElementById('cancel-change-password');
      
      // Password strength elements
      const passwordStrength = document.querySelector('.password-strength');
      const passwordStrengthMeter = document.querySelector('.password-strength-meter');
      const reqLength = document.getElementById('req-length');
      const reqUppercase = document.getElementById('req-uppercase');
      const reqLowercase = document.getElementById('req-lowercase');
      const reqNumber = document.getElementById('req-number');
      const reqSpecial = document.getElementById('req-special');
      
      // State variables
      let isRegister = true;
      let currentUser = null;
      
      // Password validation requirements
      const passwordRequirements = {
        minLength: 8,
        hasUppercase: /[A-Z]/,
        hasLowercase: /[a-z]/,
        hasNumber: /\d/,
        hasSpecial: /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/
      };
      
      // Initialize the application
      function init() {
        // Check if user is already logged in
        const loggedInUser = localStorage.getItem('currentUser');
        if (loggedInUser) {
          currentUser = JSON.parse(loggedInUser);
          showDashboard(currentUser);
        }
        
        // Set up event listeners
        setupEventListeners();
      }
      
      // Set up all event listeners
      function setupEventListeners() {
        // Form submission
        authFormElement.addEventListener('submit', handleFormSubmit);
        
        // Toggle between register and login
        toggleLink.addEventListener('click', toggleForm);
        
        // Logout button
        logoutBtn.addEventListener('click', handleLogout);
        
        // Change password
        changePasswordBtn.addEventListener('click', showChangePasswordForm);
        passwordChangeForm.addEventListener('submit', handlePasswordChange);
        cancelChangePassword.addEventListener('click', hideChangePasswordForm);
        
        // Password strength indicator
        passwordInput.addEventListener('input', updatePasswordStrength);
      }
      
      // Handle form submission (register/login)
      function handleFormSubmit(e) {
        e.preventDefault();
        
        const username = usernameInput.value.trim();
        const email = emailInput.value.trim();
        const password = passwordInput.value;
        const confirmPassword = confirmPasswordInput.value;
        
        // Validate inputs
        if (!validateInputs(username, email, password, confirmPassword)) {
          return;
        }
        
        // Show loading state
        setLoadingState(true);
        
        // Simulate API call delay
        setTimeout(() => {
          if (isRegister) {
            handleRegistration(username, email, password);
          } else {
            handleLogin(username, password);
          }
          
          setLoadingState(false);
        }, 1000);
      }
      
      // Validate form inputs
      function validateInputs(username, email, password, confirmPassword) {
        // Clear previous messages
        clearMessage();
        
        // Check if username is provided
        if (!username) {
          showMessage('Please enter a username.', 'error');
          return false;
        }
        
        // Check if email is provided and valid (for registration)
        if (isRegister) {
          if (!email) {
            showMessage('Please enter an email address.', 'error');
            return false;
          }
          
          if (!isValidEmail(email)) {
            showMessage('Please enter a valid email address.', 'error');
            return false;
          }
        }
        
        // Check if password is provided
        if (!password) {
          showMessage('Please enter a password.', 'error');
          return false;
        }
        
        // For registration, validate password strength
        if (isRegister && !isPasswordStrong(password)) {
          showMessage('Please ensure your password meets all requirements.', 'error');
          return false;
        }
        
        // For registration, check if passwords match
        if (isRegister && password !== confirmPassword) {
          showMessage('Passwords do not match.', 'error');
          return false;
        }
        
        return true;
      }
      
      // Handle user registration
      function handleRegistration(username, email, password) {
        // Check if username already exists
        if (getUserByUsername(username)) {
          showMessage('Username already exists. Please choose a different one.', 'error');
          return;
        }
        
        // Check if email already exists
        if (getUserByEmail(email)) {
          showMessage('Email address is already registered.', 'error');
          return;
        }
        
        // Create new user
        const newUser = {
          username,
          email,
          password: hashPassword(password),
          createdAt: new Date().toISOString()
        };
        
        // Save user to localStorage
        saveUser(newUser);
        
        // Show success message
        showMessage('Registration successful! You can now login.', 'success');
        
        // Switch to login form
        toggleForm();
      }
      
      // Handle user login
      function handleLogin(username, password) {
        // Find user by username
        const user = getUserByUsername(username);
        
        if (!user) {
          showMessage('User not found. Please check your username or register.', 'error');
          return;
        }
        
        // Verify password
        if (user.password !== hashPassword(password)) {
          showMessage('Incorrect password. Please try again.', 'error');
          return;
        }
        
        // Login successful
        currentUser = user;
        localStorage.setItem('currentUser', JSON.stringify(currentUser));
        showDashboard(currentUser);
      }
      
      // Handle password change
      function handlePasswordChange(e) {
        e.preventDefault();
        
        const currentPassword = document.getElementById('current-password').value;
        const newPassword = document.getElementById('new-password').value;
        const confirmNewPassword = document.getElementById('confirm-new-password').value;
        
        // Validate current password
        if (hashPassword(currentPassword) !== currentUser.password) {
          showMessage('Current password is incorrect.', 'error');
          return;
        }
        
        // Validate new password
        if (!isPasswordStrong(newPassword)) {
          showMessage('Please ensure your new password meets all requirements.', 'error');
          return;
        }
        
        // Check if new passwords match
        if (newPassword !== confirmNewPassword) {
          showMessage('New passwords do not match.', 'error');
          return;
        }
        
        // Update password
        currentUser.password = hashPassword(newPassword);
        updateUser(currentUser);
        localStorage.setItem('currentUser', JSON.stringify(currentUser));
        
        // Show success message and return to dashboard
        showMessage('Password updated successfully!', 'success');
        hideChangePasswordForm();
      }
      
      // Handle user logout
      function handleLogout() {
        currentUser = null;
        localStorage.removeItem('currentUser');
        showAuthForm();
        clearForm();
      }
      
      // Toggle between register and login forms
      function toggleForm() {
        isRegister = !isRegister;
        
        if (isRegister) {
          formTitle.textContent = 'Create Account';
          submitBtn.textContent = 'Create Account';
          toggleLink.textContent = 'Already have an account? Login here';
          emailInput.parentElement.style.display = 'block';
          confirmPasswordInput.parentElement.style.display = 'block';
          document.querySelector('.password-requirements').style.display = 'block';
        } else {
          formTitle.textContent = 'Login to Your Account';
          submitBtn.textContent = 'Login';
          toggleLink.textContent = "Don't have an account? Register here";
          emailInput.parentElement.style.display = 'none';
          confirmPasswordInput.parentElement.style.display = 'none';
          document.querySelector('.password-requirements').style.display = 'none';
        }
        
        clearForm();
        clearMessage();
      }
      
      // Show authentication form
      function showAuthForm() {
        authForm.classList.remove('hidden');
        dashboard.classList.add('hidden');
        changePasswordForm.classList.add('hidden');
      }
      
      // Show dashboard
      function showDashboard(user) {
        authForm.classList.add('hidden');
        dashboard.classList.remove('hidden');
        changePasswordForm.classList.add('hidden');
        
        userDisplay.textContent = user.username;
        dashboardUsername.textContent = user.username;
        dashboardEmail.textContent = user.email;
        accountCreated.textContent = new Date(user.createdAt).toLocaleDateString();
      }
      
      // Show change password form
      function showChangePasswordForm() {
        dashboard.classList.add('hidden');
        changePasswordForm.classList.remove('hidden');
        clearMessage();
      }
      
      // Hide change password form
      function hideChangePasswordForm() {
        changePasswordForm.classList.add('hidden');
        dashboard.classList.remove('hidden');
        passwordChangeForm.reset();
      }
      
      // Update password strength indicator
      function updatePasswordStrength() {
        const password = passwordInput.value;
        
        // Check each requirement
        const isLengthValid = password.length >= passwordRequirements.minLength;
        const hasUppercase = passwordRequirements.hasUppercase.test(password);
        const hasLowercase = passwordRequirements.hasLowercase.test(password);
        const hasNumber = passwordRequirements.hasNumber.test(password);
        const hasSpecial = passwordRequirements.hasSpecial.test(password);
        
        // Update requirement indicators
        updateRequirementIndicator(reqLength, isLengthValid);
        updateRequirementIndicator(reqUppercase, hasUppercase);
        updateRequirementIndicator(reqLowercase, hasLowercase);
        updateRequirementIndicator(reqNumber, hasNumber);
        updateRequirementIndicator(reqSpecial, hasSpecial);
        
        // Calculate strength score
        let strength = 0;
        if (isLengthValid) strength++;
        if (hasUppercase) strength++;
        if (hasLowercase) strength++;
        if (hasNumber) strength++;
        if (hasSpecial) strength++;
        
        // Update strength meter
        updateStrengthMeter(strength);
      }
      
      // Update individual requirement indicator
      function updateRequirementIndicator(element, isValid) {
        if (isValid) {
          element.classList.remove('invalid');
          element.classList.add('valid');
          element.innerHTML = '<i class="fas fa-check-circle"></i> ' + element.textContent.substring(element.textContent.indexOf(' ') + 1);
        } else {
          element.classList.remove('valid');
          element.classList.add('invalid');
          element.innerHTML = '<i class="fas fa-circle"></i> ' + element.textContent.substring(element.textContent.indexOf(' ') + 1);
        }
      }
      
      // Update password strength meter
      function updateStrengthMeter(strength) {
        // Remove all strength classes
        passwordStrength.classList.remove('weak', 'medium', 'strong');
        
        if (strength === 0) {
          passwordStrengthMeter.style.width = '0%';
        } else if (strength <= 2) {
          passwordStrength.classList.add('weak');
        } else if (strength <= 4) {
          passwordStrength.classList.add('medium');
        } else {
          passwordStrength.classList.add('strong');
        }
      }
      
      // Check if password meets all strength requirements
      function isPasswordStrong(password) {
        return (
          password.length >= passwordRequirements.minLength &&
          passwordRequirements.hasUppercase.test(password) &&
          passwordRequirements.hasLowercase.test(password) &&
          passwordRequirements.hasNumber.test(password) &&
          passwordRequirements.hasSpecial.test(password)
        );
      }
      
      // Validate email format
      function isValidEmail(email) {
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(email);
      }
      
      // Show message to user
      function showMessage(msg, type) {
        message.textContent = msg;
        message.className = 'message ' + type;
      }
      
      // Clear message
      function clearMessage() {
        message.textContent = '';
        message.className = 'message';
      }
      
      // Clear form inputs
      function clearForm() {
        authFormElement.reset();
        updatePasswordStrength();
      }
      
      // Set loading state for buttons
      function setLoadingState(isLoading) {
        if (isLoading) {
          submitBtn.classList.add('loading');
          submitBtn.disabled = true;
        } else {
          submitBtn.classList.remove('loading');
          submitBtn.disabled = false;
        }
      }
      
      // User management functions
      function saveUser(user) {
        const users = JSON.parse(localStorage.getItem('users') || '{}');
        users[user.username] = user;
        localStorage.setItem('users', JSON.stringify(users));
      }
      
      function getUserByUsername(username) {
        const users = JSON.parse(localStorage.getItem('users') || '{}');
        return users[username];
      }
      
      function getUserByEmail(email) {
        const users = JSON.parse(localStorage.getItem('users') || '{}');
        for (const username in users) {
          if (users[username].email === email) {
            return users[username];
          }
        }
        return null;
      }
      
      function updateUser(updatedUser) {
        const users = JSON.parse(localStorage.getItem('users') || '{}');
        users[updatedUser.username] = updatedUser;
        localStorage.setItem('users', JSON.stringify(users));
      }
      
      // Simple hashing function (for demo purposes only - not secure for production)
      function hashPassword(password) {
        let hash = 0;
        for (let i = 0; i < password.length; i++) {
          const char = password.charCodeAt(i);
          hash = ((hash << 5) - hash) + char;
          hash = hash & hash; // Convert to 32bit integer
        }
        return hash.toString();
      }
      
      // Initialize the application
      init();
    })();
  </script>
  
  <!-- Font Awesome for icons -->
  <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
</body>
</html>
