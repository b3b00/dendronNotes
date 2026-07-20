<div id="readability-page-1" class="page"><div data-hpc="true" containertiming="hpc"><article itemprop="text">
<p dir="auto"><code>csharp.nvim</code> is a Neovim plugin written in Lua, powered by <a href="https://github.com/OmniSharp/omnisharp-roslyn">omnisharp-roslyn</a>, that aims to enhance the development experience for .NET developers.</p>
<p dir="auto"><strong>🚧 NOTE: This plugin is in early development stage.</strong></p>
<p dir="auto"><h2 tabindex="-1" dir="auto">Prerequisites</h2><a id="user-content-prerequisites" aria-label="Permalink: Prerequisites" href="#prerequisites"></a></p>
<ul dir="auto">
<li>Locally Install <a href="https://github.com/sharkdp/fd#installation">fd</a>.</li>
</ul>
<p dir="auto"><h3 tabindex="-1" dir="auto">Roslyn LSP Specific Prerequisites</h3><a id="user-content-roslyn-lsp-specific-prerequisites" aria-label="Permalink: Roslyn LSP Specific Prerequisites" href="#roslyn-lsp-specific-prerequisites"></a></p>
<p dir="auto">If your going to use Roslyn the LSP needs to be installed manually.</p>
<ol dir="auto">
<li>Navigate to <a href="https://dev.azure.com/azure-public/vside/_artifacts/feed/vs-impl" rel="nofollow">https://dev.azure.com/azure-public/vside/_artifacts/feed/vs-impl</a> to see the latest package feed for <code>Microsoft.CodeAnalysis.LanguageServer</code></li>
<li>Locate the version matching your OS + Arch and click to open it. For example, <code>Microsoft.CodeAnalysis.LanguageServer.linux-x64</code> matches Linux-based OS in x64 architecture. Note that some OS/Arch specific packages may have an extra version ahead of the "core" non specific package.</li>
<li>On the package page, click the "Download" button to begin downloading its <code>.nupkg</code></li>
<li>Unzip the <code>.nupkg</code> file.</li>
<li>Copy the contents of <code>&lt;zip root&gt;/content/LanguageServer/&lt;yourArch/*</code> to a folder of your choice (e.g., the nvim data directory <code>$XDG_DATA_HOME/csharp/roslyn-lsp/</code>)</li>
<li>To test it is working, <code>cd</code> into the aforementioned roslyn directory and invoke <code>dotnet Microsoft.CodeAnalysis.LanguageServer.dll --version</code>. It should output server's version.</li>
</ol>
<p dir="auto"><h2 tabindex="-1" dir="auto">🚀 Installation</h2><a id="user-content--installation" aria-label="Permalink: 🚀 Installation" href="#-installation"></a></p>
<p dir="auto">Using lazy.nvim:</p>
<div dir="auto" data-snippet-clipboard-copy-content="{
  &quot;iabdelkareem/csharp.nvim&quot;,
  dependencies = {
    &quot;williamboman/mason.nvim&quot;, -- Required, automatically installs omnisharp
    &quot;mfussenegger/nvim-dap&quot;,
    &quot;Tastyep/structlog.nvim&quot;, -- Optional, but highly recommended for debugging
  },
  config = function ()
      require(&quot;mason&quot;).setup() -- Mason setup must run before csharp, only if you want to use omnisharp
      require(&quot;csharp&quot;).setup()
  end
}"><pre>{
  <span><span>"</span>iabdelkareem/csharp.nvim<span>"</span></span>,
  <span>dependencies</span> <span>=</span> {
    <span><span>"</span>williamboman/mason.nvim<span>"</span></span>, <span><span>--</span> Required, automatically installs omnisharp</span>
    <span><span>"</span>mfussenegger/nvim-dap<span>"</span></span>,
    <span><span>"</span>Tastyep/structlog.nvim<span>"</span></span>, <span><span>--</span> Optional, but highly recommended for debugging</span>
  },
  <span>config</span> <span>=</span> <span>function</span> ()
      <span>require</span>(<span><span>"</span>mason<span>"</span></span>).<span>setup</span>() <span><span>--</span> Mason setup must run before csharp, only if you want to use omnisharp</span>
      <span>require</span>(<span><span>"</span>csharp<span>"</span></span>).<span>setup</span>()
  <span>end</span>
}</pre></div>
<p dir="auto"><h2 tabindex="-1" dir="auto">⚙ Configuration</h2><a id="user-content--configuration" aria-label="Permalink: ⚙ Configuration" href="#-configuration"></a></p>
<div dir="auto" data-snippet-clipboard-copy-content="-- These are the default values
{
    lsp = {
        -- Sets if you want to use omnisharp as your LSP
        omnisharp = {
        -- When set to false, csharp.nvim won't launch omnisharp automatically.
            enable = true,
            -- When set, csharp.nvim won't install omnisharp automatically. Instead, the omnisharp instance in the cmd_path will be used.
            cmd_path = nil,
            -- The default timeout when communicating with omnisharp
            default_timeout = 1000,
            -- Settings that'll be passed to the omnisharp server
            enable_editor_config_support = true,
            organize_imports = true,
            load_projects_on_demand = false,
            enable_analyzers_support = true,
            enable_import_completion = true,
            include_prerelease_sdks = true,
            analyze_open_documents_only = false,
            enable_package_auto_restore = true,
            -- Launches omnisharp in debug mode
            debug = false,
        }
        -- Sets if you want to use roslyn as your LSP
        roslyn = {
            -- When set to true, csharp.nvim will launch roslyn automatically.
            enable = false,
            -- Path to the roslyn LSP see 'Roslyn LSP Specific Prerequisites' above.
            cmd_path = nil,
        }
        -- The capabilities to pass to the omnisharp server
        capabilities = nil,
        -- on_attach function that'll be called when the LSP is attached to a buffer
        on_attach = nil
    },
    logging = {
        -- The minimum log level.
        level = &quot;INFO&quot;,
    },
    dap = {
        -- When set, csharp.nvim won't launch install and debugger automatically. Instead, it'll use the debug adapter specified.
        --- @type string?
        adapter_name = nil,
    }
}"><pre><span><span>--</span> These are the default values</span>
{
    <span>lsp</span> <span>=</span> {
        <span><span>--</span> Sets if you want to use omnisharp as your LSP</span>
        <span>omnisharp</span> <span>=</span> {
        <span><span>--</span> When set to false, csharp.nvim won't launch omnisharp automatically.</span>
            <span>enable</span> <span>=</span> <span>true</span>,
            <span><span>--</span> When set, csharp.nvim won't install omnisharp automatically. Instead, the omnisharp instance in the cmd_path will be used.</span>
            <span>cmd_path</span> <span>=</span> <span>nil</span>,
            <span><span>--</span> The default timeout when communicating with omnisharp</span>
            <span>default_timeout</span> <span>=</span> <span>1000</span>,
            <span><span>--</span> Settings that'll be passed to the omnisharp server</span>
            <span>enable_editor_config_support</span> <span>=</span> <span>true</span>,
            <span>organize_imports</span> <span>=</span> <span>true</span>,
            <span>load_projects_on_demand</span> <span>=</span> <span>false</span>,
            <span>enable_analyzers_support</span> <span>=</span> <span>true</span>,
            <span>enable_import_completion</span> <span>=</span> <span>true</span>,
            <span>include_prerelease_sdks</span> <span>=</span> <span>true</span>,
            <span>analyze_open_documents_only</span> <span>=</span> <span>false</span>,
            <span>enable_package_auto_restore</span> <span>=</span> <span>true</span>,
            <span><span>--</span> Launches omnisharp in debug mode</span>
            <span>debug</span> <span>=</span> <span>false</span>,
        }
        <span><span>--</span> Sets if you want to use roslyn as your LSP</span>
        <span>roslyn</span> <span>=</span> {
            <span><span>--</span> When set to true, csharp.nvim will launch roslyn automatically.</span>
            <span>enable</span> <span>=</span> <span>false</span>,
            <span><span>--</span> Path to the roslyn LSP see 'Roslyn LSP Specific Prerequisites' above.</span>
            <span>cmd_path</span> <span>=</span> <span>nil</span>,
        }
        <span><span>--</span> The capabilities to pass to the omnisharp server</span>
        <span>capabilities</span> <span>=</span> <span>nil</span>,
        <span><span>--</span> on_attach function that'll be called when the LSP is attached to a buffer</span>
        <span>on_attach</span> <span>=</span> <span>nil</span>
    },
    <span>logging</span> <span>=</span> {
        <span><span>--</span> The minimum log level.</span>
        <span>level</span> <span>=</span> <span><span>"</span>INFO<span>"</span></span>,
    },
    <span>dap</span> <span>=</span> {
        <span><span>--</span> When set, csharp.nvim won't launch install and debugger automatically. Instead, it'll use the debug adapter specified.</span>
        <span><span>---</span><span> @type</span> <span>string</span><span>?</span></span>
        <span>adapter_name</span> <span>=</span> <span>nil</span>,
    }
}</pre></div>
<p dir="auto"><h2 tabindex="-1" dir="auto">🌟 Features</h2><a id="user-content--features" aria-label="Permalink: 🌟 Features" href="#-features"></a></p>
<p dir="auto"><h3 tabindex="-1" dir="auto">Automatically Installs and Configures LSP</h3><a id="user-content-automatically-installs-and-configures-lsp" aria-label="Permalink: Automatically Installs and Configures LSP" href="#automatically-installs-and-configures-lsp"></a></p>
<p dir="auto">The plugin will automatically install the LSP <code>omnisharp</code> and configure it for use.</p>
<p dir="auto"><em><g-emoji alias="warning">⚠️</g-emoji> Remove omnisharp configuration from lspconfig as the plugin handles configuring and running omnisharp. If you prefer configuring omnisharp manually using lspconfig, disable this feature by setting lsp.enable = false in the configuration.</em></p>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">Effortless Debugging</h3><a id="user-content-effortless-debugging" aria-label="Permalink: Effortless Debugging" href="#effortless-debugging"></a></p>
<p dir="auto">The plugin will automatically install the debugger <code>netcoredbg</code> and configure it for use. The goal of this functionality is to provide an effortless debugging experience to .NET developers, all you need to do is install the plugin and execute <code>require("csharp").debug_project()</code> and the plugin will take care of the rest. To make this possible the debugger supports the following features:</p>
<ul dir="auto">
<li>Automatically detects the executable project in the solution, or let you select if there are multiple executable projects.</li>
<li>Supports <a href="https://learn.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-8.0#lsj" rel="nofollow">launch settings</a> to configure <code>environmentVariables</code>, <code>applicationUrl</code>, and <code>commandLineArgs</code>.
<ul dir="auto">
<li><em>Support is limited to launch profiles with <code>CommandName == Project</code>.</em></li>
</ul>
</li>
<li>Uses .NET CLI to build the debugee project.</li>
</ul>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://private-user-images.githubusercontent.com/13891133/306035966-d4a18920-c7b0-4960-b6d2-2cb01673a29a.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDYwMzU5NjYtZDRhMTg5MjAtYzdiMC00OTYwLWI2ZDItMmNiMDE2NzNhMjlhLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM0YWMwZGY5MDhmYTY4MzE2ZTI3MDkwMGIyOTVlZjNhNzI2MjAxNmQ3OTlmNzM2ODUwMmZjZTUwOGU2MGE0OWQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.TOmzpzntQJW5-CVG_oTcEnOA-x-6D7A43otGyOTYtNk"><img src="https://private-user-images.githubusercontent.com/13891133/306035966-d4a18920-c7b0-4960-b6d2-2cb01673a29a.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDYwMzU5NjYtZDRhMTg5MjAtYzdiMC00OTYwLWI2ZDItMmNiMDE2NzNhMjlhLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM0YWMwZGY5MDhmYTY4MzE2ZTI3MDkwMGIyOTVlZjNhNzI2MjAxNmQ3OTlmNzM2ODUwMmZjZTUwOGU2MGE0OWQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.TOmzpzntQJW5-CVG_oTcEnOA-x-6D7A43otGyOTYtNk" alt="debugging" data-animated-image=""></a></p>
<p dir="auto"><em>In the illustration above, there's a solution with 3 projects, 2 of which are executable, and only one has launch settings file.</em></p>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">Run Project</h3><a id="user-content-run-project" aria-label="Permalink: Run Project" href="#run-project"></a></p>
<p dir="auto">Similar to the debugger, the plugin exposes the function <code>require("csharp").run_project()</code> that supports selection of an executable project, launch profile, builds and runs the project.</p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://private-user-images.githubusercontent.com/13891133/305989234-aa1df4e3-d3ce-43b8-a0d5-476e1b567125.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDU5ODkyMzQtYWExZGY0ZTMtZDNjZS00M2I4LWEwZDUtNDc2ZTFiNTY3MTI1LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQxYWM2MTViZmM1YWI0NTc0MDRiMmFjZWUxZmU0YjU4MzkxZWMzMTc2NWU1N2E0NmUwN2U5YjUzMjZlZjZhY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.6Y6bKcdjXxY25lAz1uJQjypnAl39SXIHTiRC19-8sGI"><img src="https://private-user-images.githubusercontent.com/13891133/305989234-aa1df4e3-d3ce-43b8-a0d5-476e1b567125.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDU5ODkyMzQtYWExZGY0ZTMtZDNjZS00M2I4LWEwZDUtNDc2ZTFiNTY3MTI1LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQxYWM2MTViZmM1YWI0NTc0MDRiMmFjZWUxZmU0YjU4MzkxZWMzMTc2NWU1N2E0NmUwN2U5YjUzMjZlZjZhY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.6Y6bKcdjXxY25lAz1uJQjypnAl39SXIHTiRC19-8sGI" alt="run" data-animated-image=""></a></p>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">View &amp; Edit User Secrets</h3><a id="user-content-view--edit-user-secrets" aria-label="Permalink: View &amp; Edit User Secrets" href="#view--edit-user-secrets"></a></p>
<p dir="auto">You can use the <code>require("csharp").view_user_secrets()</code> to view and edit <a href="https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets" rel="nofollow">user secrets</a>.
Similar to the debugger, the plugin exposes the function <code>require("csharp").run_project()</code> that supports selection of an executable project, launch profile, builds and runs the project.</p>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">Remove Unnecessary Using Statements (Omnisharp only)</h3><a id="user-content-remove-unnecessary-using-statements-omnisharp-only" aria-label="Permalink: Remove Unnecessary Using Statements (Omnisharp only)" href="#remove-unnecessary-using-statements-omnisharp-only"></a></p>
<p dir="auto">Removes all unnecessary using statements from a document. Trigger this feature via the Command <code>:CsharpFixUsings</code> or use the Lua function below.</p>
<div dir="auto" data-snippet-clipboard-copy-content="require(&quot;csharp&quot;).fix_usings()"><pre><span>require</span>(<span><span>"</span>csharp<span>"</span></span>).<span>fix_usings</span>()</pre></div>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://private-user-images.githubusercontent.com/13891133/302615198-3902ef06-b2a0-4be8-b138-222c820cf4d6.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxOTgtMzkwMmVmMDYtYjJhMC00YmU4LWIxMzgtMjIyYzgyMGNmNGQ2LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTY2NmI5NzBiNjU4OTI5YTYzMGI5YTIxNWE2MjE5ZjFjNjRjZjhkMjY2MDM2Yjc5MGViN2E2OTkzY2MzNjQ5NDYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.AZJv2kk0exVEUK47XTytFOhYDZmMbPh8vgCxWbMEHeE"><img src="https://private-user-images.githubusercontent.com/13891133/302615198-3902ef06-b2a0-4be8-b138-222c820cf4d6.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxOTgtMzkwMmVmMDYtYjJhMC00YmU4LWIxMzgtMjIyYzgyMGNmNGQ2LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTY2NmI5NzBiNjU4OTI5YTYzMGI5YTIxNWE2MjE5ZjFjNjRjZjhkMjY2MDM2Yjc5MGViN2E2OTkzY2MzNjQ5NDYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.AZJv2kk0exVEUK47XTytFOhYDZmMbPh8vgCxWbMEHeE" alt="csharp_fix_usings" data-animated-image=""></a></p>
<p dir="auto"><em>TIP: You can run this feature automatically before a buffer is saved.</em></p>
<div dir="auto" data-snippet-clipboard-copy-content="-- Listen to LSP Attach
vim.api.nvim_create_autocmd(&quot;LspAttach&quot;, {
  callback = function (args)
    local augroup = vim.api.nvim_create_augroup(&quot;LspFormatting&quot;, {})
    vim.api.nvim_create_autocmd(&quot;BufWritePre&quot;, {
      group = augroup,
      buffer = args.buf,
      callback = function()

        -- Format the code before you run fix usings
        vim.lsp.buf.format({ timeout = 1000, async = false })

        -- If the file is C# then run fix usings
        if vim.bo[0].filetype == &quot;cs&quot; then
          require(&quot;csharp&quot;).fix_usings()
        end
      end,
    })
  end
})"><pre><span><span>--</span> Listen to LSP Attach</span>
<span>vim</span>.<span>api</span>.<span>nvim_create_autocmd</span>(<span><span>"</span>LspAttach<span>"</span></span>, {
  <span>callback</span> <span>=</span> <span>function</span> (<span>args</span>)
    <span>local</span> <span>augroup</span> <span>=</span> <span>vim</span>.<span>api</span>.<span>nvim_create_augroup</span>(<span><span>"</span>LspFormatting<span>"</span></span>, {})
    <span>vim</span>.<span>api</span>.<span>nvim_create_autocmd</span>(<span><span>"</span>BufWritePre<span>"</span></span>, {
      <span>group</span> <span>=</span> <span>augroup</span>,
      <span>buffer</span> <span>=</span> <span>args</span>.<span>buf</span>,
      <span>callback</span> <span>=</span> <span>function</span>()

        <span><span>--</span> Format the code before you run fix usings</span>
        <span>vim</span>.<span>lsp</span>.<span>buf</span>.<span>format</span>({ <span>timeout</span> <span>=</span> <span>1000</span>, <span>async</span> <span>=</span> <span>false</span> })

        <span><span>--</span> If the file is C# then run fix usings</span>
        <span>if</span> <span>vim</span>.<span>bo</span>[<span>0</span>].<span>filetype</span> <span>==</span> <span><span>"</span>cs<span>" </span></span><span>then</span>
          <span>require</span>(<span><span>"</span>csharp<span>"</span></span>).<span>fix_usings</span>()
        <span>end</span>
      <span>end</span>,
    })
  <span>end</span>
})</pre></div>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">Fix All (Omnisharp only)</h3><a id="user-content-fix-all-omnisharp-only" aria-label="Permalink: Fix All (Omnisharp only)" href="#fix-all-omnisharp-only"></a></p>
<p dir="auto">This feature allows developers to efficiently resolve a specific problem across multiple instances in the codebase (e.g., a document, project, or solution) with a single command. You can run this feature using the Command <code>:CsharpFixAll</code> or the Lua function below. When the command runs, it'll launch a dropdown menu asking you to choose the scope in which you want the plugin to search for fixes before it presents the different options to you.</p>
<div dir="auto" data-snippet-clipboard-copy-content="require(&quot;csharp&quot;).fix_all()"><pre><span>require</span>(<span><span>"</span>csharp<span>"</span></span>).<span>fix_all</span>()</pre></div>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://private-user-images.githubusercontent.com/13891133/302615192-5d815ce4-b9b1-40b9-a049-df1570bea100.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxOTItNWQ4MTVjZTQtYjliMS00MGI5LWEwNDktZGYxNTcwYmVhMTAwLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTVjMmJiYzE5YmI2NTQ1MTYxYjcyNDNiZTA4NmVhYTNmZDk3OTYxYzViN2RjYmZhNjJkM2M4OThiMTVmZDRmM2QmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.zRloq8hX26zSiUXrShOQ96By2zThli5sF3AGTzTXiC8"><img src="https://private-user-images.githubusercontent.com/13891133/302615192-5d815ce4-b9b1-40b9-a049-df1570bea100.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxOTItNWQ4MTVjZTQtYjliMS00MGI5LWEwNDktZGYxNTcwYmVhMTAwLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTVjMmJiYzE5YmI2NTQ1MTYxYjcyNDNiZTA4NmVhYTNmZDk3OTYxYzViN2RjYmZhNjJkM2M4OThiMTVmZDRmM2QmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.zRloq8hX26zSiUXrShOQ96By2zThli5sF3AGTzTXiC8" alt="csharp_fix_all" data-animated-image=""></a></p>
<hr>
<p dir="auto"><h3 tabindex="-1" dir="auto">Enhanced Go-To-Definition (Decompilation Support)</h3><a id="user-content-enhanced-go-to-definition-decompilation-support" aria-label="Permalink: Enhanced Go-To-Definition (Decompilation Support)" href="#enhanced-go-to-definition-decompilation-support"></a></p>
<p dir="auto">Similar to <a href="https://github.com/Hoffs/omnisharp-extended-lsp.nvim">omnisharp-extended-lsp.nvim</a>, this feature allows developers to navigate to the definition of a symbol in the codebase with decompilation support for external code.</p>
<div dir="auto" data-snippet-clipboard-copy-content="-- Not needed with roslyn LSP, use vim.lsp.buf.definition()
require(&quot;csharp&quot;).go_to_definition()"><pre><span><span>--</span> Not needed with roslyn LSP, use vim.lsp.buf.definition()</span>
<span>require</span>(<span><span>"</span>csharp<span>"</span></span>).<span>go_to_definition</span>()</pre></div>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://private-user-images.githubusercontent.com/13891133/302615187-1b8ea6fa-6d6b-4cab-a060-2123247b0d74.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxODctMWI4ZWE2ZmEtNmQ2Yi00Y2FiLWEwNjAtMjEyMzI0N2IwZDc0LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJjZmViY2RjN2U4YzY0NjY1YWRkNDQ3MjlmYjY1YWQxYTEzMzhiMzM2YTc5NWIyNDY5ODc4ZDRmYTFiMTVhNGQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.u9HlpdUIbQCdYTH6-QgC96WIO0hwYzJ8dCn53e45Bwg"><img src="https://private-user-images.githubusercontent.com/13891133/302615187-1b8ea6fa-6d6b-4cab-a060-2123247b0d74.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODQ1NTQ4MTYsIm5iZiI6MTc4NDU1NDUxNiwicGF0aCI6Ii8xMzg5MTEzMy8zMDI2MTUxODctMWI4ZWE2ZmEtNmQ2Yi00Y2FiLWEwNjAtMjEyMzI0N2IwZDc0LmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA3MjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNzIwVDEzMzUxNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJjZmViY2RjN2U4YzY0NjY1YWRkNDQ3MjlmYjY1YWQxYTEzMzhiMzM2YTc5NWIyNDY5ODc4ZDRmYTFiMTVhNGQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmdpZiJ9.u9HlpdUIbQCdYTH6-QgC96WIO0hwYzJ8dCn53e45Bwg" alt="csharp_go_to_definition" data-animated-image=""></a></p>
<hr>
<p dir="auto"><h2 tabindex="-1" dir="auto">🪲 Reporting Bugs</h2><a id="user-content-beetle-reporting-bugs" aria-label="Permalink: :beetle: Reporting Bugs" href="#beetle-reporting-bugs"></a></p>
<ol dir="auto">
<li>Set debug level to TRACE via the configurations.</li>
<li>Reproduce the issue.</li>
<li>Open an issue in <a href="https://github.com/iabdelkareem/csharp.nvim/issues">GitHub</a> with the following details:
<ul dir="auto">
<li>Description of the bug.</li>
<li>How to reproduce.</li>
<li>Relevant logs, if possible.</li>
</ul>
</li>
</ol>
<p dir="auto"><h2 tabindex="-1" dir="auto">😍 Contributing &amp; Feature Suggestions</h2><a id="user-content-heart_eyes-contributing--feature-suggestions" aria-label="Permalink: :heart_eyes: Contributing &amp; Feature Suggestions" href="#heart_eyes-contributing--feature-suggestions"></a></p>
<p dir="auto">I'd love to hear your ideas and suggestions for new features! Feel free to create an issue and share your thoughts. We can't wait to discuss them and bring them to life!</p>
</article></div></div>