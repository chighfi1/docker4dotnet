# escape=`

# shows a static html website
# 

# v the base image (notice it's from microsoft, not dockerhub, microsoft has its own registry)
FROM mcr.microsoft.com/windows/servercore/iis:windowsservercore
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
# Configuring IIS to write log entries to a single file in a known path                              v where do we write all the logs to
RUN Set-WebConfigurationProperty -pspath 'MACHINE/WEBROOT/APPHOST' -filter 'system.applicationHost/log' -name 'centralLogFileMode' -value 'CentralW3C'; `
    Set-WebConfigurationProperty -pspath 'MACHINE/WEBROOT/APPHOST' -filter 'system.applicationHost/log/centralW3CLogFile' -name 'truncateSize' -value 4294967295; `
    Set-WebConfigurationProperty -pspath 'MACHINE/WEBROOT/APPHOST' -filter 'system.applicationHost/log/centralW3CLogFile' -name 'period' -value 'MaxSize'; `
    Set-WebConfigurationProperty -pspath 'MACHINE/WEBROOT/APPHOST' -filter 'system.applicationHost/log/centralW3CLogFile' -name 'directory' -value 'c:\iislog'
# when you run a container from this image, you're going to run the powershell command, and you're going to do the startup script 
ENTRYPOINT ["powershell", "C:\\start.ps1"]

#copy in the startup script
COPY start.ps1 C:\
#copy in content of the website
COPY index.html C:\inetpub\wwwroot