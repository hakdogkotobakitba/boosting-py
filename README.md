# boosting-py
import requests

# Function to read credentials from the file
def read_credentials():
    credentials = []
    with open('credentials.txt', 'r') as file:
        for line in file:
            uid|password = line.strip().split('|')  # Splitting by the pipe '|'
            credentials.append((uid, password))
    return credentials

# Facebook login URL for mobile version
login_url = 'https://m.facebook.com/login.php'

# Facebook post URL
post_url = 'https://m.facebook.com/{uid}/feed'

# Function to login with UID and password and post on Facebook
def login_and_post(uid, password):
    # Start a session to maintain login
    session = requests.Session()

    # Prepare login data
    login_data = {
        'email': uid,  # Use UID as email for mobile login
        'pass': password,
        'login': 'Log In'
    }

    # Send login request to Facebook
    login_response = session.post(login_url, data=login_data)

    # Check if login was successful (e.g., by checking for "Home" in the response)
    if 'Home' in login_response.text:
        print(f"Logged in successfully with UID: {uid}")

        # Data for posting (You can modify the post message here)
        post_data = {
            'xhpc_message': 'Automated post using UID and password!',
            'privacy': 'EVERYONE'  # Set privacy to Everyone for public post
        }

        # Make a post request to the user's feed
        post_response = session.post(post_url.format(uid=uid), data=post_data)

        if post_response.status_code == 200:
            print(f"Post successfully shared from UID: {uid}")
        else:
            print(f"Failed to post from UID: {uid}")
    else:
        print(f"Login failed for UID: {uid}")

# Read credentials from the file
credentials = read_credentials()

# Loop through each account's credentials to login and post
for uid, password in credentials:
    login_and_post(uid, password)
    
