#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["src/Applications/GamingWebApp/GamingWebApp.csproj", "src/Applications/GamingWebApp/"]
RUN dotnet restore "src/Applications/GamingWebApp/GamingWebApp.csproj"
COPY . .
WORKDIR "/src/src/Applications/GamingWebApp"
RUN dotnet build "GamingWebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GamingWebApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GamingWebApp.dll"]
