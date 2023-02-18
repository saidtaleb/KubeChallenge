FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
# Install NodeJs
RUN apt-get update && \
apt-get install -y wget && \
apt-get install -y gnupg2 && \
wget -qO- https://deb.nodesource.com/setup_18.x | bash - && \
apt-get install -y build-essential nodejs
# End Install
WORKDIR /src
COPY ["/Calicot.WebApp/Calicot.WebApp.csproj", "./Calicot.WebApp/"]
COPY ["/Calicot.Shared/.", "./Calicot.Shared/"]
RUN dotnet restore "Calicot.WebApp/Calicot.WebApp.csproj"
COPY . .
WORKDIR "/src/Calicot.WebApp"
RUN dotnet build "Calicot.WebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Calicot.WebApp.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Calicot.WebApp.dll"]
