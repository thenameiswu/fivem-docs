```lua
version '1.0'

author 'Wu <wu#9194>'
description 'Resource Boilerplate'
repository 'https://github.com/thenameiswu/...'

fx_version 'cerulean'
games { 'gta5' }

lua54 'yes'
use_fxv2_oal 'yes'

--[[
    ui_page 'frontend/build/index.html'

    files {
        'frontend/build/index.html',
        'frontend/build/**/*'
    }
]]

server_scripts {
    --'@oxmysql/lib/MySQL.lua',
    'src/lua/cl_*.lua',
    'build/server.js',
}

client_scripts {
    'src/lua/cl_*.lua',
    'build/client.js',
}
```

_A valid folder structure is included when you navigate to resources > \[assets\]. Remember that all folders should be lowercased._
