FROM mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2022
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Remove-Item -Recurse C:\inetpub\wwwroot\*

RUN Install-WindowsFeature -Name Web-ASP; Install-WindowsFeature -Name Web-ISAPI-Ext

WORKDIR /inetpub/wwwroot

COPY VCASoftware.Evaluation.AspClassic/Contacts/ .