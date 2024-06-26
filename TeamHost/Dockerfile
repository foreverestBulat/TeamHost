#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TeamHost/TeamHost.csproj", "TeamHost/"]
COPY ["WebTeamHost.App/WebTeamHost.App.csproj", "WebTeamHost.App/"]
COPY ["WebTeamHost.Domain/WebTeamHost.Domain.csproj", "WebTeamHost.Domain/"]
COPY ["WebTeamHost.Shared/WebTeamHost.Shared.csproj", "WebTeamHost.Shared/"]
COPY ["WebTeamHost.Infrastructure/WebTeamHost.Infrastructure.csproj", "WebTeamHost.Infrastructure/"]
COPY ["WebTeamHost.Persistance/WebTeamHost.Persistance.csproj", "WebTeamHost.Persistance/"]
COPY ["WebTeamHost.SignalR/WebTeamHost.SignalR.csproj", "WebTeamHost.SignalR/"]
RUN dotnet restore "./TeamHost/./TeamHost.csproj"
COPY . .
WORKDIR "/src/TeamHost"
RUN dotnet build "./TeamHost.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./TeamHost.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TeamHost.dll"]