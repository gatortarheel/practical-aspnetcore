# Options Walkthrough 

> https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-6.0

## Step 1
Add the config values to appsettings.config
```json
  "RabbitMQOptions": {
    "HostName": "longawsguid.mq.us-east-1.amazonaws.com",
    "UserName": "demoadmin",
    "Password": "bork!bork!bork!",
    "VirtualHost": "/",
    "Port": "5671"
  },
```
---

## Step 2
Create a class to hold the options
> Note that `RabbitMQOptions` == `RabbitMQOptions`
```CSharp
    public class RabbitMQOptions
    {
        public const string RabbitMQSection = "RabbitMQOptions";

        public string HostName { get; set; } = String.Empty;
        public string UserName { get; set; } = String.Empty;
        public string Password { get; set; } = String.Empty;
        public string VirtualHost { get; set; } = String.Empty;
        public int Port { get; set; } = 0;
    }
```
---

## Step 3

In `Program.cs`, inject the configuration into the dependency container
```CSharp
builder.Services.Configure<RabbitMQOptions>(
    builder.Configuration.GetSection(RabbitMQOptions.RabbitMQSection));
```
---

### Step 4

In the controller, add the dependency.
```CSharp
        private readonly RabbitMQOptions _rabbitMQOptions;
        public QueueController(IOptions<RabbitMQOptions> rabbitMQOptions)
        {
            _rabbitMQOptions = rabbitMQOptions.Value;
        }
```
---
### Step 5 
Use the dependency
```CSharp
 var factory = new ConnectionFactory()
            {
                HostName = _rabbitMQOptions.HostName,
                UserName = _rabbitMQOptions.UserName,
                Password = _rabbitMQOptions.Password,
                Port = _rabbitMQOptions.Port,
                VirtualHost = _rabbitMQOptions.VirtualHost,
                Ssl = new SslOption(_rabbitMQOptions.HostName, enabled: true)
            };
```