# base image
# https://mcr.microsoft.com/en-us/artifact/mar/windows/nanoserver/tags
# https://github.com/OpenListTeam/OpenList/releases/latest
# https://github.com/OpenListTeam/OpenList/releases/download/v4.0.8/openlist-windows-amd64.zip
FROM mcr.microsoft.com/windows/servercore:ltsc2022 AS extract
# set the working directory
WORKDIR /app
# Download the latest OpenList release
ADD https://github.com/OpenListTeam/OpenList/releases/latest/download/openlist-windows-amd64.zip .
# Unzip the OpenList release using PowerShell (available in servercore)
RUN powershell -Command "Expand-Archive -Path openlist-windows-amd64.zip -DestinationPath . -Force; Remove-Item openlist-windows-amd64.zip"

# Final stage using nano server for smaller image size
FROM mcr.microsoft.com/windows/nanoserver:ltsc2022 AS prod
WORKDIR /app
# Copy the extracted files from the previous stage
COPY --from=extract /app .
# Set the entry point to the OpenList executable
ENTRYPOINT ["openlist.exe","server","--no-prefix"]
