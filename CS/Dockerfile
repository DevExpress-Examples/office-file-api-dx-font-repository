# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0-bullseye-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
 
# Install the latest version of font libraries
RUN apt update &&\
         apt install -y libc6 libicu-dev libfontconfig1

FROM mcr.microsoft.com/dotnet/sdk:7.0-bullseye-slim AS build




## Build runtime image
#FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base
#WORKDIR /app
#EXPOSE 80
#EXPOSE 443
#
## Install dependencies
#RUN apk update && apk upgrade
#RUN apk add icu-libs icu-data-full fontconfig
#
#ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false
#
#FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build




WORKDIR /src
 
# Specify your DevExpress NuGet Feed URL as the package source
RUN dotnet nuget add source https://nuget.devexpress.com/{your-authorization-token}/api
# Copy the project file
COPY ["DXRepository.csproj", "."]
 
# Restore as distinct layers
RUN dotnet restore "./DXRepository.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "DXRepository.csproj" -c Release -o /app/build
 
# Publish a release
RUN dotnet publish "DXRepository.csproj" -c Release -o /app/publish
 
FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
 
# Define the entry point for the application
ENTRYPOINT ["dotnet", "DXRepository.dll"]