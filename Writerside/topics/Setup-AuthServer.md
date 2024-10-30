# Setup AuthServer

```C#
.AddAuthServer();

.AddOptions<DiscoveryDocument>().Configure(options => );
.AddOptions<JwksDocument>().Configure(options => );
.AddOptions<UserInteraction>().Configure(options => );

.AddSingleton<IDistributedCache, InMemoryCache>();
.AddScoped<IUserClaimService, UserClaimService>();
.AddScoped<IAuthenticatedUserAccessor, AuthenticatedUserAccessor>();
.AddScoped<IAuthenticationContextReferenceResolver,
    AuthenticationContextReferenceResolver>();

.UseAuthServer();
```