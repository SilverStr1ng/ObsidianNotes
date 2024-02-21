
在 `node_modules/app_builder_lib/out/targets/nsis/NsisTarget.js`文件, 在`executeMakensis`中添加参数
```JavaScript
async executeMakensis(
    async executeMakensis(defines, commands, script) {

        const args = this.options.warningsAsErrors === false ? [] : ["-WX"];

        args.push("-INPUTCHARSET", "UTF8");

        for (const name of Object.keys(defines)) {

            const value = defines[name];

            if (value == null) {

                args.push(`-D${name}`);

            }

            else {

                args.push(`-D${name}=${value}`);

            }

        }

        for (const name of Object.keys(commands)) {

            const value = commands[name];

            if (Array.isArray(value)) {

                for (const c of value) {

                    args.push(`-X${name} ${c}`);

                }

            }

            else {

                args.push(`-X${name} ${value}`);

            }

        }

        args.push("-");

        if (this.packager.debugLogger.isEnabled) {

            this.packager.debugLogger.add("nsis.script", script);

        }

        const nsisPath = await (0, nsisUtil_1.NSIS_PATH)();

        const command = path.join(nsisPath, process.platform === "darwin" ? "mac" : process.platform === "win32" ? "Bin" : "linux", process.platform === "win32" ? "makensis.exe" : "makensis");

        // if (process.platform === "win32") {

        // fix for an issue caused by virus scanners, locking the file during write

        // https://github.com/electron-userland/electron-builder/issues/5005

        await ensureNotBusy(commands["OutFile"].replace(/"/g, ""));

        // }

        await (0, builder_util_1.spawnAndWrite)(command, args, script, {

            // we use NSIS_CONFIG_CONST_DATA_PATH=no to build makensis on Linux, but in any case it doesn't use stubs as MacOS/Windows version, so, we explicitly set NSISDIR

            env: { ...process.env, NSISDIR: nsisPath },

            cwd: nsisUtil_1.nsisTemplatesDir,

        });

    }
)
```