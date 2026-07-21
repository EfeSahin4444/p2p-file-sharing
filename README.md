================================================================================
CMP2204 - P2P FILE SHARING APPLICATION
================================================================================

1. PREREQUISITES AND SETUP
--------------------------------------------------------------------------------
Before running the application, ensure that Python 3.9 (or newer) is installed on your system. 
The project strictly relies on the external 'pyDes' library for the Data Encryption Standard (DES) operations.

Install the required library via pip:
> pip install pyDes

Ensure that both testing machines (peers) are connected to the same Local Area Network (LAN) and that their network profiles are set to "Private". 

2. HOW TO RUN THE APPLICATION
--------------------------------------------------------------------------------
1. Open your terminal or command prompt.
2. Navigate to the directory containing the python file.
3. Run the script: 
   > python lvbel_c++_p2p_app.py
4. Upon startup, the console will prompt you to enter a username.
5. Next, it will ask for a file path to host. 
   - If you provide a valid file path, the program will automatically split it into 3 chunks and host them in the "shared_chunks" directory.
   - If you leave it empty and press Enter, you will enter the network purely as a downloader/browser.
6. Use the CLI menu to view available contents (Option 1), download chunks (Option 2), view transfer history (Option 3), or exit (Option 4).

3. HOW THE PROGRAM WORKS (ARCHITECTURE)
--------------------------------------------------------------------------------
- Chunk Announcer & Discovery (UDP): The application binds to UDP Port 6000. It broadcasts a JSON payload containing the user's hosted chunks to the LAN (255.255.255.255) every 8 seconds. 
- Chunk Uploader & Downloader (TCP): All actual file transfers occur over TCP Port 6001. 
- Unencrypted Transfer: The client requests a chunk, and the server sends the Base64 encoded raw bytes in a JSON payload.
- Encrypted Transfer (Secure): When security is requested, the client and server perform a Diffie-Hellman Key Exchange to generate a shared secret, used by the pyDes library to encrypt chunk data using DES (ECB mode).
- Logging: Every successful download and upload is logged with a timestamp into "download_history.txt" and "upload_history.txt".

4. KNOWN LIMITATIONS
--------------------------------------------------------------------------------
- Firewall Restrictions: Windows Defender Firewall may block the incoming UDP/TCP packets. Ensure Python is allowed in Firewall settings.
- Static Subnet Bound: The discovery mechanism uses local broadcast, so peers must be on the same subnet.
- Hardcoded Chunk Slicing: The application logic is designed to split a given hosted file into exactly 3 chunks.
