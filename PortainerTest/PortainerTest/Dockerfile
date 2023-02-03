#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["PortainerTest/PortainerTest.csproj", "PortainerTest/"]
RUN dotnet restore "PortainerTest/PortainerTest.csproj"
COPY . .
WORKDIR "/src/PortainerTest"
RUN dotnet build "PortainerTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PortainerTest.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PortainerTest.dll"]