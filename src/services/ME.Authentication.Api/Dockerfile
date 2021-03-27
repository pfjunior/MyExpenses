#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/services/ME.Authentication.Api/ME.Authentication.Api.csproj", "src/services/ME.Authentication.Api/"]
RUN dotnet restore "src/services/ME.Authentication.Api/ME.Authentication.Api.csproj"
COPY . .
WORKDIR "/src/src/services/ME.Authentication.Api"
RUN dotnet build "ME.Authentication.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ME.Authentication.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ME.Authentication.Api.dll"]