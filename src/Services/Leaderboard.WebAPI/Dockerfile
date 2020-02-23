#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["src/Services/Leaderboard.WebAPI/Leaderboard.WebAPI.csproj", "src/Services/Leaderboard.WebAPI/"]
COPY ["src/Extensions/Microsoft.AspNetCore.HealthChecks/Microsoft.AspNetCore.HealthChecks.csproj", "src/Extensions/Microsoft.AspNetCore.HealthChecks/"]
COPY ["src/Extensions/Microsoft.Extensions.HealthChecks/Microsoft.Extensions.HealthChecks.csproj", "src/Extensions/Microsoft.Extensions.HealthChecks/"]
RUN dotnet restore "src/Services/Leaderboard.WebAPI/Leaderboard.WebAPI.csproj"
COPY . .
WORKDIR "/src/src/Services/Leaderboard.WebAPI"
RUN dotnet build "Leaderboard.WebAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Leaderboard.WebAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Leaderboard.WebAPI.dll"]
