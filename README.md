# -wifi-password-viewer-
“Python script to view saved Wi-Fi passwords (Windows only)” - 
import subprocess

def get_saved_wifi_passwords():
    try:
        profiles_output = subprocess.check_output("netsh wlan show profiles", shell=True).decode("utf-8", errors="ignore")
        profiles = [line.split(":")[1].strip() for line in profiles_output.split("\n") if "All User Profile" in line]

        print("\n🔐 Saved Wi-Fi Networks and Passwords:\n")
        for profile in profiles:
            try:
                result = subprocess.check_output(f'netsh wlan show profile name="{profile}" key=clear', shell=True).decode("utf-8", errors="ignore")
                password_lines = [line for line in result.split("\n") if "Key Content" in line]
                if password_lines:
                    password = password_lines[0].split(":")[1].strip()
                else:
                    password = "🔒 Not available"
                print(f"{profile}: {password}")
            except subprocess.CalledProcessError:
                print(f"{profile}: ❌ Error retrieving password")
    except Exception as e:
        print(f"⚠️ Failed to retrieve Wi-Fi profiles: {e}")

get_saved_wifi_passwords()
