# DockerFiddle

DockerFiddle for quick setup of some important dockers that I use for testing
- [Keycloak](https://hub.docker.com/r/jboss/keycloak/)
- [Postgres](https://hub.docker.com/_/postgres)

# Keycloak with Postgres
 Composition of keycloak and postgres. It starts a keycloak server backed by postgres.
  - Access admin console from `http://localhost:8080/` -> username: admin, password: admin@123
  
 After setting up the realms, roles, user and client, you can test :
- Open `http://localhost:8080/auth/realms/<realm_name>/` -> shows the realm config settings
- Through Commandline, you can test your client settings (note: I am using [jq](https://stedolan.github.io/jq/) which is JSON processor)
```
export access_token=$(\
    curl -X POST http://localhost:8080/auth/realms/<realm_name>/protocol/openid-connect/token \
    -H 'content-type: application/x-www-form-urlencoded' \
    -d 'username=<username>&password=<password>&grant_type=password&client_id=<client-id>' | jq --raw-output '.access_token' \
 )
```
This will set the access-token environment variable and then you authenticate to the server with this token

```
curl -v -X GET \
  http://localhost:8080/service/secured \
  -H "Authorization: Bearer "$access_token
```
 
