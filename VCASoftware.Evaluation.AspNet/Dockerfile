FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 5174


ENV ASPNETCORE_URLS=http://+:5174


# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["./VCASoftware.Evaluation.AspNet/VCASoftware.Evaluation.AspNet.Web/VCASoftware.Evaluation.AspNet.Web.csproj", "VCASoftware.Evaluation.AspNet.Web/"]
RUN dotnet restore "VCASoftware.Evaluation.AspNet.Web/VCASoftware.Evaluation.AspNet.Web.csproj"
WORKDIR "/src/VCASoftware.Evaluation.AspNet.Web"
COPY . .
RUN dotnet build "VCASoftware.Evaluation.AspNet.Web.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "VCASoftware.Evaluation.AspNet.Web.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "VCASoftware.Evaluation.AspNet.Web.dll"]
