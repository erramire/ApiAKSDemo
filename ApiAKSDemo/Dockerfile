#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["ApiAKSDemo/ApiAKSDemo.csproj", "ApiAKSDemo/"]
RUN dotnet restore "ApiAKSDemo/ApiAKSDemo.csproj"
COPY . .
WORKDIR "/src/ApiAKSDemo"
RUN dotnet build "ApiAKSDemo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ApiAKSDemo.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV CONSTRING_APPINSIGHTS="InstrumentationKey=2bc41059-16b2-4318-aa9e-6ed17a4362f4;IngestionEndpoint=https://southcentralus-0.in.applicationinsights.azure.com/"
ENTRYPOINT ["dotnet", "ApiAKSDemo.dll"]