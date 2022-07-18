# escape=`
#two step docer file because there are two  FROM statements

#1st is the builder stage
#   copies in all the source code for the application so my source folder has everything I need

FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 AS builder

WORKDIR C:\src
COPY src .
# runs a nuget restore
RUN .\build-web.bat

# app image
# this is the runtime, doesn't have any of the SDK tools
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

#creating a working directorfor my web application
WORKDIR C:\web-app
# create new app called app using the default web site, using the path we made
RUN New-WebApplication -Name 'app' -Site 'Default Web Site' -PhysicalPath C:\web-app
# Copy everything from the builder stage everything that comes out in the publishedwebsites direcotry
COPY --from=builder C:\out\_PublishedWebsites\SignUp.Web .