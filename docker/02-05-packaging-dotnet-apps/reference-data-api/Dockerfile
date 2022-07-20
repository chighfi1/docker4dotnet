FROM mcr.microsoft.com/dotnet/core/sdk:3.1 as builder
#Source files that contain all my dependencies in application I'm using
WORKDIR /src
COPY src/SignUp.Entities/SignUp.Entities.csproj ./SignUp.Entities/
COPY src/SignUp.Api.ReferenceData/SignUp.Api.ReferenceData.csproj ./SignUp.Api.ReferenceData/

#Switch to the directory for my API
WORKDIR /src/SignUp.Api.ReferenceData
RUN dotnet restore

#copy in the rest of the source code and run donet publish
COPY src /src
RUN dotnet publish -c Release -o /out SignUp.Api.ReferenceData.csproj

# app image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

# v so that docker knows that the application inside the container is listening on that port and it will use that information
# if I run a container with the publish flag
EXPOSE 80
#setting my working directory
WORKDIR /app
ENTRYPOINT ["dotnet", "/app/SignUp.Api.ReferenceData.dll"]

#Copying from builder stage all the output and any content from my application
COPY --from=builder /out/ .
#Copying a specific configuration file with details to connect to my database
COPY ./docker/02-05-packaging-dotnet-apps/reference-data-api/appsettings.json .