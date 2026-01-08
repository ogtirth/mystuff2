
1. **Find the bad module**  
   Run this command (admin cmd):  
   `%windir%\system32\inetsrv\appcmd.exe list modules`  
   This lists all global modules. Look for weird ones – unsigned DLLs, random names (like miniscreen.dll, cachsess.dll, ManagedEngine.dll, or anything not Microsoft/trusted).  
   Check the main config file too: Open `%windir%\system32\inetsrv\config\applicationhost.config` in notepad, search for <globalModules> – spot suspicious entries (path to DLL in weird spots like C:\ProgramData\...).

2. **Unregister/remove the module**  
   For each bad one (replace "BadModuleName" with the name from list):  
   `%windir%\system32\inetsrv\appcmd.exe uninstall module "BadModuleName"`  
   Or use IIS Manager GUI: Open IIS Manager > Server level > Modules > right-click bad one > Remove (do global and any site-level).

3. **Delete the malicious DLL file**  
   Go to the path from the config (usually %windir%\system32\inetsrv\ or %windir%\SysWOW64\inetsrv\ or ProgramData folders). Delete the DLL (e.g., miniscreen.dll or whatever). If locked, stop IIS first: `iisreset /stop`.

4. **Clean extras**  
   - Scan whole server with good AV (Windows Defender full scan or ESET/Malwarebytes).  
   - Look for webshells (random .asp/.php in web roots), rogue users (net user cmd), or backdoors like Rungan in C:\ProgramData\Microsoft\DRM\log\.  
   - Delete any suspicious files/folders.

5. **Finish up**  
   - Patch everything: Windows updates, fix the vuln that let them in (SQL injection common).  
   - Change all admin passwords.  
   - Restart IIS: `iisreset /start`.  
   - Check Google Search Console for spam pages – request removal/reindex.

