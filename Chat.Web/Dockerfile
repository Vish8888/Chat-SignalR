# Build stage
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

# Copy csproj and restore separately for better caching
COPY ["Chat.Web/Chat.Web.csproj", "Chat.Web/"]
RUN dotnet restore "Chat.Web/Chat.Web.csproj"

# Copy everything and publish
COPY . .
WORKDIR /src/Chat.Web
RUN dotnet publish "Chat.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS runtime
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:80
EXPOSE 80

COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "Chat.Web.dll"]