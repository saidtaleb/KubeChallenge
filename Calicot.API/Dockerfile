FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["/Calicot.API/Calicot.API.csproj", "./Calicot.API/"]
COPY ["/Calicot.Shared/.", "./Calicot.Shared/"]
RUN dotnet restore "Calicot.API/Calicot.API.csproj"
COPY . .
WORKDIR "/src/Calicot.API"
RUN dotnet build "Calicot.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Calicot.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS final
WORKDIR /app
EXPOSE 5000
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Calicot.API.dll"]
