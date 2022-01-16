# uprofile.nvim

Dynamically toggle nvim configuration based on user profiles.

## Quick start

```lua
local uprofile = require("uprofile")
uprofile.setup()
```

## Usage

You can set the profile using a environment variable. The default variable is `NVIM_PROFILE`.

You can then add to your .bashrc the following line:

```bash
export NVIM_PROFILE=personal
```

and in another machine

```bash
export NVIM_PROFILE=work
```

In your nvim you can then toggle configuration like this:

```lua
local uprofile = require("uprofile")
uprofile.setup()

-- Install Plugins
packer.startup(function()
	local use = use or use
	use({ "wbthomason/packer.nvim" }) -- updates package manager

    -- use onedark for personal and material for work
    use({ 
        uprofile.with_profile_table({
            personal = "navarasu/onedark.nvim",
            work = "marko-cerovac/material.nvim",
        })
    })
end)
```

Another example is to conditionally enable different lsp servers based on the profile

```lua
local servers = user_profile.with_profile_table({
    -- default profile is a fallback, if there is no match, `default` is returned
	default = { "efm", "sumneko_lua", "tsserver", "eslint", "gopls", "clangd", "rust_analyzer", "pyright" },
	work = { "efm", "sumneko_lua", "tsserver", "eslint" },
})
```

## API

### uprofile.setup(config)

Example:

```lua
uprofile.setup({
    -- if not set, this will be active
    default_profile = "personal",
    
    env_name = "NVIM_PROFILE",
})
```

### uprofile.get_active_profile()

Returns the current profile

### uprofile.with_profile_fn(profile_name, fn, ...)

Only executes `fn` if `profile_name` matches the the current profile

Example:

```lua
uprofile.with_profile_fn("work", function()
    print("work profile is enabled")
end)

-- or

-- special `any` profile will run for all profiles
uprofile.with_profile_fn("any", function()
    print(uprofile.get_active_profile(), " is enabled")
end)
```

### uprofile.with_profile_table(tbl)

index `tbl` with the current active profile

