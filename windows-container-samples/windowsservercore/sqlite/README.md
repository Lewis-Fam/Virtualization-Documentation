# Description:

Create a demo container with SQLite 3.12.0 software. 

This dockerfile is for demonstration purposes and may require modification for production use. 

# Environment:

Windows Server Core Base OS Image

# Usage:

**Docker Build**

```
docker build -t sqlite:latest .
```

**Docker Run** 

This will run a container, display the SQLite version, and then exit. Modify the Dockerfile appropriately for application use.

```
docker run -it sqlite
```

## Dockerfile Details:
```
# This dockerfile utilizes components licensed by their respective owners/authors.
# Prior to utilizing this file or resulting images please review the respective licenses at: https://www.sqlite.org/copyright.html

FROM windowsservercore

LABEL Description="SQLite" Vendor="SQLite" Version="3.12.0"

RUN powershell -Command \
	$ErrorActionPreference = 'Stop'; \
	Invoke-WebRequest -Method Get -Uri https://www.sqlite.org/2016/sqlite-dll-win64-x64-3120000.zip -OutFile c:\sqlite-dll-win64-x64-3120000.zip ; \
	Expand-Archive -Path c:\sqlite-dll-win64-x64-3120000.zip -DestinationPath c:\sqlite ; \
	Remove-Item c:\sqlite-dll-win64-x64-3120000.zip -Force

RUN powershell -Command \
	$ErrorActionPreference = 'Stop'; \
	Invoke-WebRequest -Method Get -Uri "https://www.sqlite.org/2016/sqlite-tools-win32-x86-3120000.zip" -OutFile c:\sqlite-tools-win32-x86-3120000.zip ; \
	Expand-Archive -Path c:\sqlite-tools-win32-x86-3120000.zip -DestinationPath c:\sqlite ; \
	Copy-Item -Path c:\sqlite\sqlite-tools-win32-x86-3120000\*.* c:\sqlite ; \
	Remove-Item c:\sqlite\sqlite-tools-win32-x86-3120000 -Recurse -Force ; \
	Remove-Item c:\sqlite-tools-win32-x86-3120000.zip -Force

RUN setx /M PATH "%PATH%;c:\sqlite"

CMD ["sqlite3.exe"]
```

