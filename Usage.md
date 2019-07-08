
#### Usage:

##### To activate and communicate with the "native" (sort of native...) Electron API include the ElectronNET.API NuGet package in your ASP.NET Core app.

```PM
PM> Install-Package ElectronNET.API
```
#### Program.cs

You start Electron.NET up with an UseElectron WebHostBuilder-Extension.

```cs
public static IWebHost BuildWebHost(string[] args)
{
    return WebHost.CreateDefaultBuilder(args)
        .UseElectron(args)
        .UseStartup<Startup>()
        .Build();
}
```

#### Startup.cs

Open the Electron Window in the Startup.cs file:
```cs
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });

    // Open the Electron-Window here
    Task.Run(async () => await Electron.WindowManager.CreateWindowAsync());
}
```
##### Please note: Currently it is important to use ASP.NET Core with MVC. If you are working with the dotnet CLI, use

```dotnet
dotnet new mvc

```
