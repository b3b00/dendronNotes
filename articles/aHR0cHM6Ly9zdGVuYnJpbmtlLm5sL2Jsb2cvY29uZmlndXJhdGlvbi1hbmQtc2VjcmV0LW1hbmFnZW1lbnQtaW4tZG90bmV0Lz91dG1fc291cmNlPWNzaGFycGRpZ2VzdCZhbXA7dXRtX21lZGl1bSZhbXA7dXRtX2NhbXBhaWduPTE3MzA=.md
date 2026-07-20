<div id="readability-page-1" class="page"><div><div><h2>Table Of Contents</h2><nav id="TableOfContents"><ul><li><a href="#introduction">Introduction</a></li><li><a href="#configuration-in-net">Configuration in .NET</a><ul><li><a href="#the-basics">The basics</a></li><li><a href="#access-structured-data">Access structured data</a></li><li><a href="#how-does-this-all-work">How does this all work?</a></li><li><a href="#treating-configuration-as-code">Treating configuration as code</a></li></ul></li><li><a href="#options-pattern">Options pattern</a><ul><li><a href="#dependency-injection">Dependency injection</a></li><li><a href="#validation">Validation</a></li><li><a href="#configuration-lifetimes">Configuration lifetimes</a></li></ul></li><li><a href="#secret-management-during-development">Secret management during development</a><ul><li><a href="#the-user-secrets-configuration-provider">The user secrets configuration provider</a></li><li><a href="#using-user-secrets">Using user secrets</a></li><li><a href="#setting-up-a-project-that-uses-user-secrets">Setting up a project that uses user-secrets</a></li></ul></li><li><a href="#my-template-for-configuration-management">My template for configuration management</a><ul><li><a href="#appsettingsjson">appsettings.json</a></li><li><a href="#appsettingsdevelopmentjson">appsettings.Development.json</a></li><li><a href="#user-secrets">User secrets</a></li></ul></li><li><a href="#using-azure-to-store-configuration">Using Azure to store configuration</a><ul><li><a href="#storing-secrets-in-azure-key-vault">Storing secrets in Azure Key Vault</a></li><li><a href="#connecting-to-azure-with-managed-identities">Connecting to Azure with Managed Identities</a></li><li><a href="#storing-configuration-in-azure-app-configuration">Storing configuration in Azure App Configuration</a></li></ul></li><li><a href="#finishing-up">Finishing up</a><ul><li><a href="#links-to-the-demo">Links to the demo</a></li><li><a href="#links-to-the-official-documentation">Links to the official documentation</a></li></ul></li></ul></nav></div><div><p>info</p><div><p>This blog post is a companion to my
<a href="https://sessionize.com/s/sander-ten-brinke/keep-it-secret-keep-it-safe-with-.net/48314" target="_blank">Keep it secret, keep it safe with .NET</a> talk! If you are unable to visit a session of this talk, you can read this blog post instead! This way as many people as possible can learn about .NET’s configuration system and how to keep secrets safe!</p><p>My talk offers some more in-depth information, so if you want to know more, take a look at my
<a href="https://stenbrinke.nl/speaking">Speaking</a> page to see when and where I will be giving this talk again! You can also
<a href="https://stenbrinke.nl/about/#contact-details">contact me</a> if you want me to give this talk at your event!</p></div></div><h2 id="introduction">Introduction
<a href="#introduction">
<span>Link to heading</span></a></h2><p>If you’ve been writing code for a while, you probably have used configuration in some way. Think of feature flags, logging settings, authentication settings, etc… Maybe you’ve used a configuration file containing some settings for your application, or maybe you’ve used environment variables. Maybe you’ve used both!</p><p>It’s also likely that you interacted with secrets, which I also consider to be part of a configuration system. Think of connection strings and API keys. These should always be secure!</p><p>Configuration in .NET has changed dramatically since the introduction of .NET Core. Gone are the days of using multiple <code>Web.config</code> files, we now have a much more flexible system. However, a flexible system can also be a complex system. That is why I wanted to create a session and blog post in which you’ll learn how .NET’s configuration system works, and how you can use it optimally. You’ll also learn how to keep your secrets safe, both locally and in production, using the power of the Azure cloud!</p><div><p>info</p><div><p>This blog post contains <strong>everything</strong> that you need to know about configuration and secret management in .NET. It doesn’t cover every single detail, but it does cover everything I believe a .NET developer should know. Consider this a handy pocketguide you can fall back on, or send to others when they have questions about configuration and secret management in .NET.</p><p>I think this blog post is very useful because it takes <strong>multiple hours</strong> to <em>find</em> and fully read Microsoft’s documentation about these topics. If you want to learn more, you’ll find links to the official documentation at the end of this blog post.</p></div></div><p>Let’s get started!</p><h2 id="configuration-in-net">Configuration in .NET
<a href="#configuration-in-net">
<span>Link to heading</span></a></h2><p>.NET’s configuration system is very flexible! You can use multiple configuration providers, each one possibly having a different configuration format:</p><figure><picture><source srcset="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/dark/configuration-overview.webp" alt="An image showing an overview of .NET's configuration system with the IConfiguration interface and multiple providers." media="(prefers-color-scheme: dark)"><img src="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/light/configuration-overview.webp" alt="An image showing an overview of .NET's configuration system with the IConfiguration interface and multiple providers."></picture><figcaption><p>An overview of .NET’s configuration system.<br><a href="https://learn.microsoft.com/dotnet/core/extensions/configuration?WT.mc_id=DT-MVP-5005050#concepts-and-abstractions" target="_blank">Taken from the official docs</a></p></figcaption></figure><p>Other sources can be files like <code>.xml</code>, <code>.ini</code> and so much more. You can even connect your configuration system to the cloud, which we’ll do later on!</p><h2 id="the-basics">The basics
<a href="#the-basics">
<span>Link to heading</span></a></h2><p>As you can see, all your configuration can be accessed using the <code>IConfiguration</code> interface. With this, you can retrieve your values in a strongly typed manner. An example:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> IConfiguration Configuration { <span>get</span>; <span>set</span>; }
</span></span><span><span>
</span></span><span><span><span>public</span> <span>string</span> GetApiKey()
</span></span><span><span>{
</span></span><span><span>    <span>// GetValue&lt;&gt; allows you to pass in the return type</span>
</span></span><span><span>    <span>string</span> method1 = Configuration.GetValue&lt;<span>string</span>&gt;(<span>"ApiKey"</span>); 
</span></span><span><span>
</span></span><span><span>    <span>// The indexer variant always returns a string</span>
</span></span><span><span>    <span>string</span> method2 = Configuration[<span>"ApiKey"</span>]; 
</span></span><span><span>
</span></span><span><span>    <span>return</span> method1;
</span></span><span><span>}
</span></span></code></pre></div><p>You’ll notice that we are not specifying what provider should be used to retrieve the <code>ApiKey</code>. This is because it shouldn’t matter; <code>IConfiguration</code> hides all this complexity from us and thus creates flexibility. The configuration system decides what provider to use based on the order of the providers. We’ll talk more about that later!</p><h2 id="access-structured-data">Access structured data
<a href="#access-structured-data">
<span>Link to heading</span></a></h2><p>A very powerful feature of .NET’s configuration system is that it supports structured data. This is very useful because it allows you to group related configuration values. All providers support structured data, but if you’ve ever worked with an ASP.NET Core project, you’ll likely recognize the most common one, which is <code>appsettings.json</code>. The following JSON is an example of such a file:</p><div><pre tabindex="0"><code data-lang="json"><span><span>{
</span></span><span><span>  <span>"Logging"</span>: {
</span></span><span><span>    <span>"LogLevel"</span>: {
</span></span><span><span>      <span>"Default"</span>: <span>"Information"</span>,
</span></span><span><span>      <span>"Microsoft.AspNetCore"</span>: <span>"Warning"</span>
</span></span><span><span>    }
</span></span><span><span>  },
</span></span><span><span>  <span>"AllowedHosts"</span>: <span>"*"</span>,
</span></span><span><span>
</span></span><span><span>  <span>"ConnectionStrings"</span>: {
</span></span><span><span>    <span>"Database"</span>: <span>"CONNECTIONSTRING_HERE"</span>
</span></span><span><span>  },
</span></span><span><span>
</span></span><span><span>  <span>"Features"</span>: {
</span></span><span><span>    <span>"EnableNewUI"</span>: <span>false</span>
</span></span><span><span>  }
</span></span><span><span>}
</span></span></code></pre></div><p>You can imagine the root object and the <code>Logging</code>, <code>ConnectionStrings</code>and <code>Features</code> sections as “structured data”.</p><p>To interact with these sections in .NET you can use the following code.</p><div><p>warning</p><p>The following code is not the best way to interact with structured data! We’ll talk about better ways (using the Options pattern)
<a href="#options-pattern">later on</a>.</p></div><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> IConfiguration Configuration { <span>get</span>; <span>set</span>; }
</span></span><span><span>
</span></span><span><span><span>// ...</span>
</span></span><span><span>
</span></span><span><span><span>public</span> IConfigurationSection GetFeaturesSection()
</span></span><span><span>{
</span></span><span><span>    <span>// GetSection returns null when the section can't be found</span>
</span></span><span><span>    <span>var</span> method1 = Configuration.GetSection(<span>"Features"</span>);
</span></span><span><span>
</span></span><span><span>    <span>// GetRequiredSection throws when the section can't be found.</span>
</span></span><span><span>    <span>// ALWAYS prefer this over GetSection to prevent nasty bugs!</span>
</span></span><span><span>    <span>var</span> method2 = Configuration.GetRequiredSection(<span>"Features"</span>);
</span></span><span><span>
</span></span><span><span>    <span>return</span> method2;
</span></span><span><span>}
</span></span><span><span>
</span></span><span><span><span>public</span> <span>bool</span> GetEnableNewUI()
</span></span><span><span>{
</span></span><span><span>    <span>return</span> Configuration.GetValue&lt;<span>bool</span>&gt;(<span>"Features:EnableNewUI"</span>);
</span></span><span><span>}
</span></span></code></pre></div><p><code>IConfigurationSection</code> provides the same API as <code>IConfiguration</code>, so you could call <code>featuresSection.GetValue&lt;bool&gt;("EnableNewUI")</code> to get the value of that section. You can also directly access a configuration value that exists inside a section using a <code>:</code>, which you can see in use in the <code>GetEnableNewUI</code> method.</p><div><p>info</p><p>Structured data is not limited to JSON files. Every provider supports it, though the syntax to specify a section might differ. For example, to provide a value for <code>EnableNewUI</code> using an environment variable, you’ll have to create one named <code>Features__EnableNewUI</code>.</p></div><h2 id="how-does-this-all-work">How does this all work?
<a href="#how-does-this-all-work">
<span>Link to heading</span></a></h2><p>We’ve been using <code>IConfiguration</code> in some examples now. You might be wondering how this all works under the hood; how do you create an instance of <code>IConfiguration</code>, and how do you set it up? Let’s take a look!</p><p>To create an instance of <code>IConfiguration</code>, you’ll need to use the <code>ConfigurationBuilder</code> class (or another class that implements <code>IConfigurationBuilder</code>).
This class uses the Builder pattern so you can add multiple providers. In the end, you call <code>Build()</code> and end up with an <code>IConfigurationRoot</code>. This is the same thing as <code>IConfiguration</code>, but it also has a list of all the providers you added. You should never use <code>IConfigurationRoot</code> directly because you shouldn’t access the providers under the hood. An example:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>var</span> builder = <span>new</span> ConfigurationBuilder();
</span></span><span><span>
</span></span><span><span>builder.AddJsonFile(<span>"sharedsettings.json"</span>);
</span></span><span><span>builder.AddJsonFile(<span>"appsettings.json"</span>);
</span></span><span><span>builder.AddEnvironmentVariables();
</span></span><span><span><span>// And so on...</span>
</span></span><span><span>
</span></span><span><span>IConfigurationRoot configuration = builder.Build();
</span></span></code></pre></div><p>The order of these providers is very important because it’s a layered system. Take a look at the following image:</p><figure><picture><source srcset="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/dark/configuration-providers-layers.webp" alt="An image showing how the importance of the order of configuration providers." media="(prefers-color-scheme: dark)"><img src="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/light/configuration-providers-layers.webp" alt="An image showing how the importance of the order of configuration providers."></picture><figcaption><p>An overview of the importance of the order of configuration providers.<br><a href="https://www.manning.com/books/asp-net-core-in-action-second-edition" target="_blank">ASP.NET Core in Action, Second Edition (Permission granted by author Andrew Lock)</a></p></figcaption></figure><p>Imagine that <code>sharedsettings.json</code> has a value for all the configuration values used by the application. <code>appsettings.json</code> and the <code>Environment variables</code> both contain a subset of these values. Because the provider for the environment variables was added last, it has the highest priority. So if you want to retrieve a configuration value called <code>ApiKey</code>, the system will first look at the environment variables. If it exists, it will be returned, even if other providers <em>also</em> contain a value for <code>ApiKey</code>. However, if the environment variables do not contain a value for <code>ApiKey</code>, it would move on to the provider that was added before it and search there, and so on.</p><h3 id="the-defaults">The defaults
<a href="#the-defaults">
<span>Link to heading</span></a></h3><p>You might be a little confused at this point. I sure was when I learned about <code>IConfigurationBuilder</code> and the importance of those layers. Why? Well, I realized that I had been using <code>IConfiguration</code> in lots of projects, but I had never heard of <code>IConfigurationBuilder</code> before. So how could I have been using <code>IConfiguration</code>?</p><p>This works because if you work on a .NET application that
<a href="https://learn.microsoft.com/dotnet/core/extensions/generic-host?WT.mc_id=DT-MVP-5005050" target="_blank">uses a Host</a>, it will set up an entire configuration system for you by default! For example,
<a href="https://learn.microsoft.com/aspnet/core/fundamentals/host/generic-host?WT.mc_id=DT-MVP-5005050" target="_blank">ASP.NET Core projects</a> and
<a href="https://learn.microsoft.com/dotnet/core/extensions/workers?WT.mc_id=DT-MVP-5005050#worker-service-template" target="_blank">worker services</a> use a <code>Host</code>, so in most projects this will be done for you! We’ll now take a look at how that works.</p><p>In a standard ASP.NET Core application, the following is set up for you.</p><table><thead><tr><th>Provider</th><th>Example</th><th>Notes</th></tr></thead><tbody><tr><td>appsettings.json</td><td><code>{ "Key": "default value" }</code></td><td></td></tr><tr><td>appsettings.<strong>{ENVIRONMENT}</strong>.json</td><td><code>{ "Key": "development value" }</code></td><td></td></tr><tr><td>User Secrets <strong>(Development)</strong></td><td><code>dotnet user-secrets set "key" "development value"</code></td><td>Can also be set in IDE’s.<br>More on this
<a href="#user-secrets">later</a>.</td></tr><tr><td>Environment variables</td><td><strong>Powershell:</strong><br><code>setx key "environment value"</code><br><strong>Bash:</strong><br><code>export key="environment value"</code></td><td>Can also be set in IDE’s.<br>Very popular with Docker/Kubernetes deployments.</td></tr><tr><td>Command line arguments</td><td><code>dotnet run --key "important value"</code></td><td>Can also be set in IDE’s.</td></tr></tbody></table><p>The item on the top has the lowest priority. So if you were to call <code>Configuration["key"]</code>, you would get <code>important value</code> as a result, even though User Secrets also provide a value.</p><div><p>info</p><p>The User secrets provider is only added when the <em>Environment</em> is set to <em>Development</em>. Environments will be handled next. User Secrets are handled in-depth
<a href="#user-secrets">later on</a>.</p></div><p>Visual Studio (and other IDE’s like JetBrains Rider) support setting environment variables/command-line arguments in your IDE when you go to the properties of the project. However, I advise <strong>against</strong> using this during development. I never use environment variables or command line arguments during development because it’s more difficult to edit these than simply opening a file. Storing them in <code>appsettings.Development.json</code> (which will be covered next) is more convenient for you and your colleagues.</p><h4 id="configuration-and-environments">Configuration and Environments
<a href="#configuration-and-environments">
<span>Link to heading</span></a></h4><p>The <em>appsettings.{<strong>ENVIRONMENT</strong>}.json</em> provider is a bit different than other providers. This is because this is dependent on
<a href="https://learn.microsoft.com/aspnet/core/fundamentals/environments?WT.mc_id=DT-MVP-5005050" target="_blank">the environment of the application</a>. The current environment of the application is read from the value of the <code>DOTNET_ENVIRONMENT</code> or <code>ASPNETCORE_ENVIRONMENT</code> environment variable. If your project is not an ASP.NET Core project, the application will only check <code>DOTNET_ENVIRONMENT</code>. ASP.NET Core projects fall back to <code>DOTNET_ENVIRONMENT</code> when <code>ASPNETCORE_ENVIRONMENT</code> doesn’t exist.</p><p>The environment will be regarded as <code>Production</code> when these environment variables do not exist.</p><p>When you create an ASP.NET Core project, a file called <code>launchSettings.json</code> will be created in the <code>Properties</code> folder. In here, you can see that the <code>ASPNETCORE_ENVIRONMENT</code> environment variable is set to <code>Development</code>:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span>{
</span></span><span><span>  <span>// Lots of other fluff has been removed from this file for the sake of brevity</span>
</span></span><span><span>  <span>"$schema"</span>: <span>"https://json.schemastore.org/launchsettings.json"</span>,
</span></span><span><span>  <span>"profiles"</span>: {
</span></span><span><span>    <span>"MY_PROJECT"</span>: {
</span></span><span><span>      <span>"commandName"</span>: <span>"MY_PROJECT"</span>,
</span></span><span><span>      <span>"applicationUrl"</span>: <span>"https://localhost:7237;http://localhost:5292"</span>,
</span></span><span><span>      <span>"environmentVariables"</span>: {
</span></span><span><span>        <span>"ASPNETCORE_ENVIRONMENT"</span>: <span>"Development"</span>
</span></span><span><span>      }
</span></span><span><span>    }
</span></span><span><span>  }
</span></span><span><span>}
</span></span></code></pre></div><p>The result of the configuration system working together with the application’s environment results in a very powerful feature because it allows you to create different configuration files for each environment.<br>You can store default values in <code>appsettings.json</code>, and override them in <code>appsettings.Development.json</code>, <code>appsettings.Test.json</code>, <code>appsettings.Staging.json</code> and <code>appsettings.Production.json</code>.</p><p>For example, let’s say you’ve finished the new design for a checkout page of a webshop. It must still be tested and reviewed by others in a test environment, but it shouldn’t go live on production yet. This sounds like a perfect use case for feature flags! You could create a feature flag called <code>EnableNewCheckoutUI</code> and set it to <code>false</code> in <code>appsettings.json</code> as the default value. Then, you could override these values in <code>appsettings.Development.json</code> and <code>appsettings.Test.json</code> so they are only enabled there:</p><div><pre tabindex="0"><code data-lang="json"><span><span><span>// appsettings.json
</span></span></span><span><span><span></span>{
</span></span><span><span>  <span>"FeatureFlags"</span>: {
</span></span><span><span>    <span>"EnableNewCheckoutUI"</span>: <span>false</span>
</span></span><span><span>  }
</span></span><span><span>}
</span></span><span><span>
</span></span><span><span><span>// appsettings.Development.json and appsettings.Test.json
</span></span></span><span><span><span></span>{
</span></span><span><span>  <span>"FeatureFlags"</span>: {
</span></span><span><span>    <span>"EnableNewCheckoutUI"</span>: <span>true</span>
</span></span><span><span>  }
</span></span><span><span>}
</span></span></code></pre></div><p>You aren’t limited to the aforementioned environment names; they are just the defaults that .NET uses. If you want to use a different name, you can do so by setting the <code>ASPNETCORE_ENVIRONMENT</code> environment variable to the name of your choice, and then create a corresponding <code>appsettings.ENV_NAME.json</code> file. The only other thing you need to do is to ensure that the environment you run your application on has <code>ASPNETCORE_ENVIRONMENT</code> or <code>DOTNET_ENVIRONMENT</code> set to the correct value.</p><h2 id="treating-configuration-as-code">Treating configuration as code
<a href="#treating-configuration-as-code">
<span>Link to heading</span></a></h2><p>Previously, I praised .NET’s configuration system and <code>IConfiguration</code> for being flexible and feature-rich. I talked about its support for structured data and retrieving values from a deeper configuration hierarchy. But did you know you can do a lot more with structured data?</p><p>Let’s say our application talks to an external API using HTTP. To do so, we need an <code>ApiUrl</code>, <code>ApiKey</code> and perhaps we want to configure a <code>TimeoutInMilliseconds</code>. From a code perspective, we might want to store these values in a class (or <code>record</code>) because they belong together:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> <span>class</span> <span>ExternalApiSettings</span>
</span></span><span><span>{
</span></span><span><span>    <span>public</span> <span>string</span> ApiUrl { <span>get</span>; <span>set</span>; }
</span></span><span><span>    <span>public</span> <span>string</span> ApiKey { <span>get</span>; <span>set</span>; }
</span></span><span><span>    <span>public</span> <span>int</span> TimeoutInMilliseconds { <span>get</span>; <span>set</span>; }
</span></span><span><span>}
</span></span></code></pre></div><p>Next, we would have an <code>ExternalApiClient</code> class which uses configuration and the class we just created:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> <span>class</span> <span>ExternalApiClient</span>
</span></span><span><span>{
</span></span><span><span>    <span>private</span> <span>readonly</span> IConfiguration _configuration;
</span></span><span><span>
</span></span><span><span>    <span>public</span> Foo(IConfiguration configuration)
</span></span><span><span>    {
</span></span><span><span>        _configuration = configuration;
</span></span><span><span>    }
</span></span><span><span>
</span></span><span><span>    <span>public</span> <span>void</span> CallExternalApi()
</span></span><span><span>    {
</span></span><span><span>        IConfigurationSection externalApiSettingsSection = _configuration.GetRequiredSection(<span>"ExternalApiSettings"</span>);
</span></span><span><span>        
</span></span><span><span>        <span>// Method 1 (Get&lt;TType&gt;() grabs the values from that section and maps them into a new instance of the provided class)</span>
</span></span><span><span>        ExternalApiSettings settings1 = externalApiSettingsSection.Get&lt;ExternalApiSettings&gt;(); 
</span></span><span><span>
</span></span><span><span>        <span>// Method 2 (Bind() expects an existing instance of a type and will map the values into that existing instance)</span>
</span></span><span><span>        ExternalApiSettings settings2 = <span>new</span>();
</span></span><span><span>        _configuration.GetRequiredSection(<span>"ExternalApiSettings"</span>).Bind(settings2);
</span></span><span><span>
</span></span><span><span>        <span>// Do something with these settings here..</span>
</span></span><span><span>    }
</span></span><span><span>}
</span></span></code></pre></div><p>This looks pretty neat, right? Instead of performing 3 calls to grab each API-related configuration property individually, we can map them into a strongly typed object. We can now treat our configuration as code! We could even create methods on our <code>ExternalApiSettings</code> class to make it even more powerful!</p><p>But, there are some big downsides with this approach.</p><h3 id="downsides">Downsides
<a href="#downsides">
<span>Link to heading</span></a></h3><ul><li>The first downside is that our <code>ExternalApiClient</code> requires an instance of <code>IConfiguration</code> to function. That’s a pretty big dependency and pretty wasteful, considering it only uses 3 configuration values! Also, this class can now access other configuration values like a connection string to a database, logging settings, feature flags, etc., even though it doesn’t need this information.</li><li>The second downside is that this class is violating the Single Responsibility Principle. It’s not only responsible for calling the external API, it’s also responsible for interacting with the configuration system to be able to call this external API.</li><li>Because this class interacts with the configuration system directly, it has a dependency on its structure and is thus tightly coupled. Any changes to the <code>ExternalApiSettings</code> configuration section (like the name or the names of its children) would cause issues at runtime.</li></ul><p>So, what can we do about this? Rewrite it in Rust 🦀? No, we can use the options pattern!</p><h2 id="options-pattern">Options pattern
<a href="#options-pattern">
<span>Link to heading</span></a></h2><p>The options pattern allows you to make even better use of .NET’s configuration system 🚀! It allows you to decouple your application from the configuration system and adds a lot of powerful features to said system, like:</p><ul><li>Dependency Injection</li><li>Validation</li><li>Different configuration lifetimes</li><li>And much more!</li></ul><p>To start using the options pattern effectively, you need to create classes/records of your configuration sections. We had already done so in the previous example, so let’s continue with that:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> <span>class</span> <span>ExternalApiSettings</span>
</span></span><span><span>{
</span></span><span><span>    <span>public</span> <span>string</span> ApiUrl { <span>get</span>; <span>set</span>; }
</span></span><span><span>    <span>public</span> <span>string</span> ApiKey { <span>get</span>; <span>set</span>; }
</span></span><span><span>    <span>public</span> <span>int</span> TimeoutInMilliseconds { <span>get</span>; <span>set</span>; }
</span></span><span><span>}
</span></span></code></pre></div><h2 id="dependency-injection">Dependency injection
<a href="#dependency-injection">
<span>Link to heading</span></a></h2><p>The options pattern works very well together with dependency injection! To do so, simply register your options class/record in the service collection. Depending on your project, your entry point for this might be the <code>Startup.cs -&gt; ConfigureServices(IServiceCollection services)</code> method, or somewhere in your <code>Program.cs</code>.</p><div><pre tabindex="0"><code data-lang="csharp"><span><span>services.Configure&lt;ExternalApiSettings&gt;(configuration); <span>// Pass in an existing instance of IConfiguration</span>
</span></span></code></pre></div><p>This will add an instance of <code>IOptions&lt;ExternalApiSettings&gt;</code> to your dependency injection container. To see the benefits of this approach, let’s take a look at how we could improve our <code>ExternalApiClient</code> from before:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> <span>class</span> <span>ExternalApiClient</span>
</span></span><span><span>{
</span></span><span><span>    <span>private</span> <span>readonly</span> ExternalApiSettings _externalApiSettings;
</span></span><span><span>
</span></span><span><span>    <span>public</span> ExternalApiClient(IOptions&lt;ExternalApiSettings&gt; options)
</span></span><span><span>    {
</span></span><span><span>        <span>// Important: The Options pattern is 'lazy'. This means that the options are only mapped when you request them by calling .Value!</span>
</span></span><span><span>        <span>// This is only done once, so you don't have to worry about performance.</span>
</span></span><span><span>        _externalApiSettings = options.Value;
</span></span><span><span>    }
</span></span><span><span>
</span></span><span><span>    <span>public</span> <span>void</span> CallExternalApi()
</span></span><span><span>    {
</span></span><span><span>        <span>// Do something with these settings here..</span>
</span></span><span><span>    }
</span></span><span><span>}
</span></span></code></pre></div><p>This has removed all the downsides from before! Our <code>ExternalApiClient</code> no longer has a dependency on <code>IConfiguration</code> and is no longer coupled to the configuration system. It also no longer has to worry about the structure of the configuration system.</p><p>You might argue that we have an indirect dependency on the configuration system because of the <code>.Configure&lt;&gt;(configuration)</code> call from before, but you aren’t required to use this method to set up your options. You can create an instance of <code>IOptions&lt;T&gt;</code> using <code>Microsoft.Extensions.Options.Options.Create()</code> if you need to create an instance manually, and you can pass in any data you want. You can even create options based on other dependencies by using the <code>Configure&lt;TDep1,...&gt;()</code> method from the <code>OptionsBuilder</code>, which will be discussed next.</p><div><p>info</p><p>You might wonder why we need to wrap our settings class with an <code>IOptions&lt;&gt;</code> interface. This is because this allows you to use some more advanced features we’ll talk about next.</p></div><h2 id="validation">Validation
<a href="#validation">
<span>Link to heading</span></a></h2><p>My favorite feature of .NET’s configuration system is how easy it is to validate your configuration! I believe this is one of the most important parts of any application, and I do not see it being used enough. The reason why I believe configuration is one of the most important parts of any application is because it houses very important settings of your application.</p><p>An incorrectly configured application could have disastrous results. Worst case, imagine if your test environment is accidentally connecting to production environment resources. Now imagine that you would test a mass-delete function, and accidentally delete all your production data. That would be a disaster!</p><p>That is why we want to validate our configuration. If our application starts with an incorrect configuration system, we want to exit immediately.</p><p>So how do we set this up? It’s easier than you think. I like using
<a href="https://www.infoworld.com/article/3543302/how-to-use-data-annotations-in-c-sharp.html" target="_blank">Data Annotations</a> for my options validations when I don’t require complex validation rules, so let’s modify our <code>ExternalApiSettings</code> like this:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>public</span> <span>class</span> <span>ExternalApiSettings</span>
</span></span><span><span>{
</span></span><span><span><span>    [Required]</span> <span>// If the ApiUrl is not set, the configuration is invalid</span>
</span></span><span><span>    <span>public</span> <span>string</span> ApiUrl { <span>get</span>; <span>set</span>; }
</span></span><span><span><span>
</span></span></span><span><span><span>    [Required]</span> <span>// If the ApiKey is not set, the configuration is invalid</span>
</span></span><span><span>    <span>public</span> <span>string</span> ApiKey { <span>get</span>; <span>set</span>; }
</span></span><span><span><span>
</span></span></span><span><span><span>    [Range(1, 1_000_00)]</span> <span>// If the TimeoutInMilliseconds is not set (Default is 0) or bigger than 100000, the configuration is invalid</span>
</span></span><span><span>    <span>public</span> <span>int</span> TimeoutInMilliseconds { <span>get</span>; <span>set</span>; }
</span></span><span><span>}
</span></span></code></pre></div><p>Now, let’s change the way we register those options in the dependency injection container:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span>services
</span></span><span><span>    .AddOptions&lt;ExternalApiSettings&gt;()
</span></span><span><span>    .BindConfiguration(<span>"ExternalApiSettings"</span>)
</span></span><span><span>    .ValidateDataAnnotations() <span>// Throws an OptionsValidationException if the configuration is invalid</span>
</span></span><span><span>    .ValidateOnStart(); <span>// Highly recommended!</span>
</span></span></code></pre></div><p>Instead of using <code>Configure&lt;TType&gt;(configuration)</code>, we now use <code>AddOptions&lt;TType&gt;()</code>. This returns an <code>OptionsBuilder</code> and allows us to use some powerful methods.</p><ol><li>The first one we use is <code>BindConfiguration()</code>. This will retrieve the <code>IConfiguration</code> from the dependency injection container and bind the section that we pass in. This is handy because we don’t have to manually pass in our configuration anymore.</li><li>Next, we call <code>ValidateDataAnnotations()</code>. This will validate our configuration section based on the attributes we set on the properties.<ol><li>Note: You need to install the
<a href="https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations" target="_blank">Microsoft.Extensions.Options.DataAnnotations</a> nuget package if you don’t have this method available to you.</li></ol></li><li>Finally, we call <code>ValidateOnStart()</code>. This one is <strong>very</strong> important! By default, your options would only get validated when you call <code>.Value</code> on them somewhere, like in a class where they are injected. This means that your application would NOT throw an error and exit on startup when your configuration is invalid! <code>ValidateOnStart()</code> will validate your configuration after your application is done bootstrapping itself.</li></ol><p>You can validate your code in many other ways, as well. You can use the <code>IValidatableOptions&lt;&gt;</code> interface to implement complex validation logic or you can call <code>Validate(Func&lt;TOptions, bool&gt; validation)</code> to write custom validation logic as part of the Options builder. You could even
<a href="https://andrewlock.net/adding-validation-to-strongly-typed-configuration-objects-using-flentvalidation/" target="_blank">integrate it with FluentValidation</a>!</p><h2 id="configuration-lifetimes">Configuration lifetimes
<a href="#configuration-lifetimes">
<span>Link to heading</span></a></h2><p>Finally, I want to talk about the lifetime of the Options pattern. <code>IOptions&lt;T&gt;</code> is a singleton. This means that if one of your
<a href="https://learn.microsoft.com/dotnet/core/extensions/configuration-providers?WT.mc_id=DT-MVP-5005050#json-configuration-provider" target="_blank">configuration providers would be updated at runtime</a>, the options will not be updated. This is because the options are only mapped once when you call <code>.Value</code> on them.</p><p>You could consider this a good thing because it means that your application will not suddenly change behavior when your configuration changes.
However, you could also say it’s a bad thing because you might not want to have to redeploy or restart your application when you change your configuration. In that case, you want to use <code>IOptionsSnapshot&lt;T&gt;</code> or <code>IOptionsMonitor&lt;T&gt;</code> instead.</p><figure><img src="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/options-lifetimes.webp" alt="Overview of the capabilities of IOptions, IOptionsSnapshot and IOptionsMonitor"><figcaption><p>Overview of the capabilities of IOptions, IOptionsSnapshot and IOptionsMonitor</p></figcaption></figure><h3 id="ioptionssnapshott">IOptionsSnapshot<t>
<a href="#ioptionssnapshott">
<span>Link to heading</span></a></t></h3><p>Instead of injecting <code>IOptions&lt;T&gt;</code> into one of your classes, you could inject <code>IOptionsSnapshot&lt;T&gt;</code> instead. This will reload that particular options type every for
<a href="https://learn.microsoft.com/dotnet/core/extensions/dependency-injection?WT.mc_id=DT-MVP-5005050#scoped" target="_blank">scope</a>. A scope in .NET is an abstract term. A scope could be an HTTP request, for example. So for each HTTP request it would reload the options and they would stay consistent for that entire request. This means that if you change your configuration, it will only be updated in a new request.</p><h3 id="ioptionsmonitort">IOptionsMonitor<t>
<a href="#ioptionsmonitort">
<span>Link to heading</span></a></t></h3><p><code>IOptionsMonitor&lt;T&gt;</code> doesn’t work with scopes. Instead, you have to call <code>.CurrentValue</code> (instead <code>.Value</code>) on it to retrieve the current version. You need to be careful with how you access your configuration, though! Imagine a scenario where your configuration changes in the middle of an HTTP request. Calling <code>.CurrentValue</code> at the beginning and end of a request would result in different values, which creates a synchronization risk. You can register a callback using <code>OnChange()</code> to be notified of these events.</p><p>This interface is most useful in a background job scenario that is only instantiated once but would benefit from being able to handle configuration changes.</p><h2 id="secret-management-during-development">Secret management during development
<a href="#secret-management-during-development">
<span>Link to heading</span></a></h2><p>If you’re going to take away one thing from this blog post, let it be the following:</p><p>If you store secrets in your git repository and your repository gets compromised, your secrets are compromised as well. I don’t think I need to explain why this is a bad thing, so how do we prevent this from happening with .NET?</p><p>By using the user secrets configuration provider!</p><h2 id="the-user-secrets-configuration-provider">The user secrets configuration provider
<a href="#the-user-secrets-configuration-provider">
<span>Link to heading</span></a></h2><p>I mentioned the user secrets configuration provider
<a href="#the-defaults">earlier</a>. This configuration provider is meant for local development <em>only</em>. It allows you to store secrets on your local machine without having to worry about them being committed to your git repository, because they are stored in a different location:</p><ul><li>Windows: <code>%APPDATA%\Microsoft\UserSecrets\&lt;user_secrets_id&gt;\secrets.json</code></li><li>Mac &amp; Linux: <code>~/.microsoft/usersecrets/&lt;user_secrets_id&gt;/secrets.json</code></li></ul><p>This file is very similar to the <code>appsettings.json</code> provider. Simply insert JSON into it and you can access it with .NET’s configuration system. When your application starts up and your <code>ASPNETCORE_ENVIRONMENT</code> or <code>DOTNET_ENVIRONMENT</code> environment variable is set to <code>Development</code>, it will automatically load the user secrets configuration provider <em>as long as your project is configured to use this provider</em>.</p><div><p>warning</p><p>Even though this provider has the name “secret” in it, be warned! The content of the <code>secrets.json</code> file is not encrypted. If you work in an environment where storing secrets on the machine itself is risky, consider using
<a href="#using-the-key-vault-during-local-development">an external secret store like the Azure KeyVault during development</a>.</p></div><p>This configuration provider can be accessed using the CLI or your favorite IDE. You might need to install the
<a href="https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets" target="_blank">Microsoft.Extensions.Configuration.UserSecrets</a> package in case you don’t use a <code>Host</code> or have a custom configuration setup.</p><h2 id="using-user-secrets">Using user secrets
<a href="#using-user-secrets">
<span>Link to heading</span></a></h2><h3 id="cli">CLI
<a href="#cli">
<span>Link to heading</span></a></h3><p>You can use the <code>dotnet</code> cli to interact with user-secrets by opening a terminal in the directory where your project’s <code>*.csproj</code> resides.</p><div><pre tabindex="0"><code data-lang="bash"><span><span><span># Only required when user-secrets aren't initialized yet</span>
</span></span><span><span>dotnet user-secrets init
</span></span><span><span>
</span></span><span><span><span># You can use structured data by using a colon (:) to separate the keys</span>
</span></span><span><span>dotnet user-secrets set <span>"ConnectionStrings:Database"</span> <span>"Data Source=..."</span>
</span></span><span><span>dotnet user-secrets set <span>"AdminPassword"</span> <span>"hunter2"</span>
</span></span><span><span><span># Other commands like "list", "remove" and "clear" are also available</span>
</span></span></code></pre></div><h3 id="visual-studio">Visual Studio
<a href="#visual-studio">
<span>Link to heading</span></a></h3><p>Right-click on a project in the Solution Explorer and select <code>Manage User Secrets</code>. A <code>secrets.json</code> file will open where you can insert your secrets.</p><h3 id="visual-studio-code">Visual Studio Code
<a href="#visual-studio-code">
<span>Link to heading</span></a></h3><p>Install the
<a href="https://marketplace.visualstudio.com/items?itemName=adrianwilczynski.user-secrets" target="_blank">.NET Core User Secrets Visual Studio Code extension</a>. Then you can right-click on a <code>*.csproj</code> file and select <code>Manage User Secrets</code>. A <code>secrets.json</code> file will open where you can insert your secrets.</p><h3 id="jetbrains-rider">JetBrains Rider
<a href="#jetbrains-rider">
<span>Link to heading</span></a></h3><p>Right-click on a project in the Solution Explorer and select <code>Tools</code> &gt; <code>Manage User Secrets</code>. A <code>secrets.json</code> file will open where you can insert your secrets.</p><h2 id="setting-up-a-project-that-uses-user-secrets">Setting up a project that uses user-secrets
<a href="#setting-up-a-project-that-uses-user-secrets">
<span>Link to heading</span></a></h2><p>One downside of using user secrets during development is that - if your project requires some secrets to run - you will need to perform some setup steps after cloning the project. I have 2 recommendations for handling this:</p><ul><li>You could create a script that retrieves the secrets from your secret storage location and then stores them in user-secrets by piping those values into <code>dotnet user-secrets set</code>. Now you only need to run this script once after cloning the project and you’re done!</li><li>Alternatively, I recommend updating your <code>README.MD</code> by including setup instructions that tell the user what user-secrets to set and where to retrieve those values from. Feel free to link to this blog post if you want to explain what user-secrets are 😉.</li></ul><h2 id="my-template-for-configuration-management">My template for configuration management
<a href="#my-template-for-configuration-management">
<span>Link to heading</span></a></h2><p>Now that we’ve covered the basics and advanced usage of .NET’s configuration system and how to incorporate local secret management, I’d like to showcase my “setup” for configuration management in a .NET project. When I create a new .NET project, I use the following setup:</p><h2 id="appsettingsjson">appsettings.json
<a href="#appsettingsjson">
<span>Link to heading</span></a></h2><p>.NET’s configuration system allows you to be very flexible with all the different providers. This is great, but can also cause confusion when your application is using configuration values you weren’t expecting, or when you can’t figure out where a specific configuration value is coming from.</p><div><p>tip</p><div><p>Use the <code>IConfigurationRoot.GetDebugView()</code> method when you’re having issues with configuration values. To do this, obtain an <code>IConfiguration</code> instance, cast it to <code>IConfigurationRoot</code> and inspect the result of <code>GetDebugView()</code>.</p><p>For more information, see Andrew Lock’s
<a href="https://andrewlock.net/debugging-configuration-values-in-aspnetcore/#exposing-the-debug-view-in-your-application" target="_blank">fantastic blog post</a> about this.</p></div></div><p>I use <code>appsettings.json</code> to store a template of all the configuration values my project uses and where the values are retrieved from. This file may also contain actual values when the <code>appsettings.json</code> file is the only provider for that configuration value. I really like this setup because it allows me to see all the configuration values my project uses in one place.</p><div><pre tabindex="0"><code data-lang="json"><span><span>{
</span></span><span><span>  <span>"Logging"</span>: {
</span></span><span><span>    <span>"LogLevel"</span>: {
</span></span><span><span>      <span>"Default"</span>: <span>"Information"</span>,
</span></span><span><span>      <span>"Microsoft.AspNetCore"</span>: <span>"Warning"</span>
</span></span><span><span>    }
</span></span><span><span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ConnectionStrings"</span>: {
</span></span><span><span>    <span>"Database"</span>: <span>"&lt;from-azure-keyvault&gt;"</span> <span>// The Azure Key Vault will be discussed in the next section
</span></span></span><span><span><span></span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ExternalApiSettings"</span>: {
</span></span><span><span>    <span>"ApiUrl"</span>: <span>"&lt;from-environment-variables&gt;"</span>,
</span></span><span><span>    <span>"ApiKey"</span>: <span>"&lt;from-azure-keyvault&gt;"</span>,
</span></span><span><span>    <span>"TimeoutInMilliseconds"</span>: <span>5000</span>
</span></span><span><span>  }
</span></span><span><span>}
</span></span></code></pre></div><h2 id="appsettingsdevelopmentjson">appsettings.Development.json
<a href="#appsettingsdevelopmentjson">
<span>Link to heading</span></a></h2><p>Next up, we have the <code>appsettings.Development.json</code> file. This file can contain configuration values that override values from <code>appsettings.json</code>, like logging settings. Furthermore, this file will and should <strong>never</strong> contain secrets! Instead, it references the user secrets configuration provider. This makes it less likely for people to insert secrets into this file because they get nudged towards using the user secrets configuration provider.</p><div><pre tabindex="0"><code data-lang="json"><span><span>{
</span></span><span><span>  <span>"Logging"</span>: {
</span></span><span><span>    <span>"LogLevel"</span>: {
</span></span><span><span>      <span>"Default"</span>: <span>"Debug"</span>, <span>// Logging settings are 100% personal preference, feel free to use whatever you like
</span></span></span><span><span><span></span>      <span>"Microsoft.AspNetCore"</span>: <span>"Warning"</span>
</span></span><span><span>    }
</span></span><span><span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ConnectionStrings"</span>: {
</span></span><span><span>    <span>"Database"</span>: <span>"&lt;from-user-secrets&gt;"</span> <span>// Every developer can use their own local database connectionstring
</span></span></span><span><span><span></span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ExternalApiSettings"</span>: {
</span></span><span><span>    <span>"ApiUrl"</span>: <span>"dev.externalapi.example.com"</span>,
</span></span><span><span>    <span>"ApiKey"</span>: <span>"&lt;from-user-secrets&gt;"</span>
</span></span><span><span>    <span>// I do not provide a value for TimeoutInMilliseconds because I'm fine with the value from appsettings.json
</span></span></span><span><span><span></span>  }
</span></span><span><span>}
</span></span></code></pre></div><h2 id="user-secrets">User secrets
<a href="#user-secrets">
<span>Link to heading</span></a></h2><p>Finally, I use the user secrets configuration provider for storing local secrets <em>and</em> overriding non-secret configuration that I don’t want to commit to the git repository, like changing the logging settings in case I need to dive deeper into a bug. If I were to change these configuration values in the <code>appsettings.Development.json</code> file, I would have to remember to revert these changes before committing my code. By using the user secrets configuration provider, I don’t have to worry about this.</p><div><pre tabindex="0"><code data-lang="json"><span><span>{
</span></span><span><span>  <span>"Logging"</span>: {
</span></span><span><span>    <span>"LogLevel"</span>: {
</span></span><span><span>      <span>"Microsoft.AspNetCore"</span>: <span>"Information"</span>
</span></span><span><span>    }
</span></span><span><span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ConnectionStrings"</span>: {
</span></span><span><span>    <span>"Database"</span>: <span>"Data Source=..."</span>
</span></span><span><span>  },
</span></span><span><span>
</span></span><span><span>  <span>"ExternalApiSettings"</span>: {
</span></span><span><span>    <span>"ApiKey"</span>: <span>"abc123def456ghi7"</span>
</span></span><span><span>  }
</span></span><span><span>}
</span></span></code></pre></div><div><p>question</p><p>What does your setup look like? What do you think of mine? Let me know in the comments below!</p></div><h2 id="using-azure-to-store-configuration">Using Azure to store configuration
<a href="#using-azure-to-store-configuration">
<span>Link to heading</span></a></h2><p>Now that we know how to store configuration and secrets locally, it’s time to talk about running your applications in real environments. There are many different ways to set up configuration and secret management for non-local environments, so it all comes down to knowing the upsides and downsides of those approaches and choosing what works best for you. In this blog post, you will learn how to use Azure to store your configuration and secrets securely.</p><h2 id="storing-secrets-in-azure-key-vault">Storing secrets in Azure Key Vault
<a href="#storing-secrets-in-azure-key-vault">
<span>Link to heading</span></a></h2><p>As we said before, you can’t use the User Secrets configuration provider in non-local environments. So we have to find a different way to store our secrets when we are deploying our applications.
<a href="https://azure.microsoft.com/services/key-vault/?WT.mc_id=DT-MVP-5005050" target="_blank">Azure Key Vault</a> is a great service for storing secrets, keys and certificates in a cheap, easy and secure way.</p><p>At the beginning of this blog post, I mentioned that it is possible to connect .NET’s configuration system to the cloud, which
<a href="https://learn.microsoft.com/aspnet/core/security/key-vault-configuration?WT.mc_id=DT-MVP-5005050" target="_blank">is possible with the Key Vault</a>. This is a great approach for secret management because you can simply treat the Key Vault as a configuration provider and no longer need to do complicated things in your release pipeline.</p><p>To add it as a configuration provider, install the
<a href="https://www.nuget.org/packages/Azure.Extensions.AspNetCore.Configuration.Secrets" target="_blank">Azure.Extensions.AspNetCore.Configuration.Secrets</a> and
<a href="https://www.nuget.org/packages/Azure.Identity" target="_blank">Azure.Identity</a> packages. Then you only need to add a few lines of code to your <code>Program.cs</code> when you create a minimal API, for example:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>using</span> Azure.Identity;
</span></span><span><span>
</span></span><span><span><span>var</span> builder = WebApplication.CreateBuilder(args);
</span></span><span><span>
</span></span><span><span>builder.Host.ConfigureAppConfiguration((context, config) =&gt;
</span></span><span><span>{
</span></span><span><span>    <span>if</span> (!context.HostingEnvironment.IsDevelopment())
</span></span><span><span>    {
</span></span><span><span>        <span>var</span> keyVaultUrl = <span>new</span> Uri(context.Configuration.GetValue&lt;<span>string</span>&gt;(<span>"KeyVaultUrl"</span>));
</span></span><span><span>        config.AddAzureKeyVault(keyVaultUrl, <span>new</span> ManagedIdentityCredential()); <span>// Other credential options are available. Managed Identities will be covered in a bit!</span>
</span></span><span><span>    }
</span></span><span><span>});
</span></span></code></pre></div><p>The Key Vault is now added as the final provider and thus has
<a href="#the-defaults">the highest priority</a>. So even if other providers have a value configured for a secret, the Key Vault will be used instead!</p><div><p>info</p><p>Structured secrets must be stored in the Key Vault with 2 dashes (<code>--</code>) instead of 2 underscores or a colon because of naming limitations. For example, <code>ExternalApiSettings--ApiKey</code> instead of <code>ExternalApiSettings:ApiKey</code> or <code>ExternalApiSettings__ApiKey</code>.</p></div><h3 id="using-the-key-vault-during-local-development">Using the Key Vault during local development
<a href="#using-the-key-vault-during-local-development">
<span>Link to heading</span></a></h3><p>Earlier in this blog post, I briefly mentioned that you might be in a scenario where you can’t store secrets on your machine because of security reasons, for example. In this case, using the Key Vault during local development would solve that problem. You can grab the code example from the previous section and modify it as follows:</p><div><pre tabindex="0"><code data-lang="csharp"><span><span><span>using</span> Azure.Identity;
</span></span><span><span>
</span></span><span><span><span>var</span> builder = WebApplication.CreateBuilder(args);
</span></span><span><span>
</span></span><span><span>builder.Host.ConfigureAppConfiguration((context, config) =&gt;
</span></span><span><span>{
</span></span><span><span>    <span>var</span> keyVaultUrl = <span>new</span> Uri(context.Configuration.GetValue&lt;<span>string</span>&gt;(<span>"KeyVaultUrl"</span>));
</span></span><span><span>    config.AddAzureKeyVault(keyVaultUrl, <span>new</span> DefaultAzureCredential()); <span>// These credential types will be covered next!</span>
</span></span><span><span>});
</span></span></code></pre></div><p>Now your application will always connect to the Key Vault, even during development. The result is that you do not need to store secrets on your machine anymore because they are always retrieved from the Key Vault. <em>A downside of this approach is that you always need an internet connection to connect to the cloud!</em></p><div><p>info</p><p>Because of the benefits that this solution brings, consider using this approach even in scenarios where storing secrets locally <em>would</em> be OK. A big benefit of this approach is that you can simply clone a project, and, as long as you have the right permissions, you can run it without having to set up any secrets on your machine because they are simply retrieved from the cloud!</p></div><h2 id="connecting-to-azure-with-managed-identities">Connecting to Azure with Managed Identities
<a href="#connecting-to-azure-with-managed-identities">
<span>Link to heading</span></a></h2><p>In the previous code examples, you’ve seen some weird credential types: <code>ManagedIdentityCredential</code> and <code>DefaultAzureCredential</code>. Before we discuss this, consider the following:</p><p><em>We want to connect to a secured secret storage provider to obtain secrets to run our application with. To connect to this provider, we’ll need to pass in some credentials so the provider can authorize our request. <strong>But those credentials are also secrets, so where do we store those</strong>? We could store them in another configuration provider, but doesn’t this defeat the whole purpose of having a secret provider? If those credentials were to leak, someone would be able to access our secrets anyway! <strong>To summarize, we’re dealing with a chicken and egg problem here.</strong></em></p><p>Luckily, some smart folks at Microsoft have figured it out! For local development, you can use the <code>DefaultAzureCredential</code> to communicate with Azure services. This credential type will try to authenticate
<a href="https://learn.microsoft.com/dotnet/api/overview/azure/identity-readme?WT.mc_id=DT-MVP-5005050#defaultazurecredential" target="_blank">using multiple methods</a>, like the Microsoft account you’re logged into your IDE with, your Azure CLI (<code>az</code>) credentials, and more.</p><figure><img src="https://stenbrinke.nl/images/blog/configuration-and-secret-management-in-dotnet/azure-identity.webp" alt="An overview of how Azure Identity works"><figcaption><p>Azure Identity uses several methods to authenticate a developer’s Azure account.<br></p></figcaption></figure><p>For production environments, Microsoft recommends
<a href="https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview" target="_blank">Managed Identities</a> to
<a href="https://learn.microsoft.com/dotnet/azure/sdk/authentication/?WT.mc_id=DT-MVP-5005050" target="_blank">authenticate with Azure resources</a>. Managed Identities are a feature of Microsoft Entra (Formerly known as Azure Active Directory) that allows you to create an identity for your application in Microsoft Entra. This identity can then be used to authenticate with other Azure services, like the Key Vault. The benefit of this approach is that you don’t need to store any credentials in your application anymore because the identity is managed by Azure AD.</p><figure><img src="https://stenbrinke.nl/images/blog//configuration-and-secret-management-in-dotnet/azure-managed-identities.webp" alt="A diagram of how Managed Identities work.</br>"></figure><div><p>info</p><p>Managed Identities can be quite difficult to grasp at first. Take a look at the bottom of the blog post for some links to more information about Managed Identities.</p></div><h2 id="storing-configuration-in-azure-app-configuration">Storing configuration in Azure App Configuration
<a href="#storing-configuration-in-azure-app-configuration">
<span>Link to heading</span></a></h2><p>Last but not least, I want to talk about Azure’s cloud offering for <em>configuration</em> management. Whilst Azure Key Vault covers <em>secret</em> management,
<a href="https://azure.microsoft.com/products/app-configuration/?WT.mc_id=DT-MVP-5005050" target="_blank">Azure App configuration</a> is a SaaS offering that will help you with managing your configuration. Both of these services work very well together when you link your Azure App Configuration to your Azure Key Vault. If you choose to use this service, you’ll only need to add the
<a href="https://learn.microsoft.com/azure/azure-app-configuration/quickstart-aspnet-core-app?WT.mc_id=DT-MVP-5005050" target="_blank">Azure App Configuration as a configuration provider</a> and then you’ll be able to use .NET’s configuration system to access all your configuration AND secrets from the cloud!</p><p>It also has many other features, so it’s worth checking out!</p><div><p>info</p></div><h2 id="finishing-up">Finishing up
<a href="#finishing-up">
<span>Link to heading</span></a></h2><p>You made it to the end! This blog post took a very long time to write, and I’m glad it’s finally done! Below you can find some more information if you want to learn more about the concepts I covered in this post. If you have any questions, feel free to leave a comment below. If you’d like to know when and where I’ll be giving the talk version of this blog post, check out my
<a href="https://stenbrinke.nl/speaking">Speaking</a> page.</p><h2 id="links-to-the-demo">Links to the demo
<a href="#links-to-the-demo">
<span>Link to heading</span></a></h2><p>I mentioned that the blog post is a session companion to one of my sessions. This session contains several demos that showcase the concepts discussed in this blog post. You can find the demos
<a href="https://github.com/sander1095/secure-secret-storage-with-asp-net-core/?WT.mc_id=DT-MVP-5005050" target="_blank">here</a>.</p><h2 id="links-to-the-official-documentation">Links to the official documentation
<a href="#links-to-the-official-documentation">
<span>Link to heading</span></a></h2><ul><li><a href="https://learn.microsoft.com/dotnet/core/extensions/configuration?WT.mc_id=DT-MVP-5005050" target="_blank">Configuration</a></li><li><a href="https://learn.microsoft.com/aspnet/core/fundamentals/configuration?WT.mc_id=DT-MVP-5005050" target="_blank">Configuration (ASP.NET Core specifics)</a></li><li><a href="https://learn.microsoft.com/aspnet/core/fundamentals/configuration/options?WT.mc_id=DT-MVP-5005050" target="_blank">Options Pattern</a></li><li><a href="https://docs.microsoft.com/aspnet/core/security/app-secrets" target="_blank">User secrets</a></li><li><a href="https://azure.microsoft.com/en-us/products/key-vault/?WT.mc_id=DT-MVP-5005050" target="_blank">Azure KeyVault</a></li><li><a href="https://devblogs.microsoft.com/devops/demystifying-service-principals-managed-identities/?WT.mc_id=DT-MVP-5005050" target="_blank">More information about Managed Identities</a></li></ul><div><hr><p>Did you enjoy this post?
<a href="https://ko-fi.com/stenbrinke" target="_blank"><img src="https://stenbrinke.nl/images/kofi_button_blue.webp" alt="Buy Me a Coffee at https://ko-fi.com/stenbrinke.nl"></a></p></div></div></div>