#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/services/PS.Payment.API/PS.Payment.API.csproj", "src/services/PS.Payment.API/"]
RUN dotnet restore "src/services/PS.Payment.API/PS.Payment.API.csproj"
COPY . .
WORKDIR "/src/src/services/PS.Payment.API"
RUN dotnet build "PS.Payment.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PS.Payment.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PS.Payment.API.dll"]