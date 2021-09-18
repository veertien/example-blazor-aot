#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

RUN dotnet workload install wasm-tools

WORKDIR /src
COPY ["Server/example1.Server.csproj", "Server/"]
COPY ["Client/example1.Client.csproj", "Client/"]
COPY ["Shared/example1.Shared.csproj", "Shared/"]
RUN dotnet restore "Server/example1.Server.csproj"
COPY . .
WORKDIR "/src/Server"
RUN dotnet build "example1.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "example1.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "example1.Server.dll"]