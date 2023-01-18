#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

RUN apt-get update -yq
RUN apt-get install curl gnupg -yq
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y nodejs

COPY ["TechRiders2023/TechRiders2023.csproj", "TechRiders2023/"]
RUN dotnet restore "TechRiders2023/TechRiders2023.csproj"
COPY . .
WORKDIR "/src/TechRiders2023"
RUN dotnet build "TechRiders2023.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TechRiders2023.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TechRiders2023.dll"]