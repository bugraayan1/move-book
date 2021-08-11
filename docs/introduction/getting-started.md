# Getting started

> **Warning:** Content on this page is outdated and requires rework. Newer version of Move IDE will be published soon. For now I recommend you to use [move-cli](https://github.com/diem/diem/tree/main/language/tools/move-cli).

---

Herhangi bir derlenmiş dilde olduğu gibi, Move uygulamalarınızı derlemek, çalıştırmak ve hatalarını ayıklamak için uygun bir araç setine ihtiyacınız vardır. Bu dil blok zincirler geliştirmek için oluşturulduğundan ve yalnızca bunların içinde kullanıldığından, komut dosyalarını zincir dışı çalıştırmak önemsiz bir görevdir: her modül bir ortam, hesap işleme ve derleme-yayınlama sistemi gerektirir.

Move modüllerinin geliştirilmesini basitleştirmek için Visual Studio Code için [Move IDE Uzantısı](https://github.com/damirka/vscode-move-ide) oluşturdum. Bu uzantı, ortam gereksinimleriyle başa çıkmanıza yardımcı olacaktır. Bu uzantının kullanılması, sizin için inşa/çalıştır ortamını idare edeceğinden şiddetle tavsiye edilir, bu nedenle CLI ile uğraşmak yerine Move dilini öğrenmeye odaklanmanıza izin verir. Bu uzantı ayrıca, uygulamalarınızın genel kullanıma sunulmadan önce hatalarının ayıklanmasına yardımcı olmak için sözdizimi vurgulamayı ve yürütücüyü içerir.

## Move IDE Yüklenmesi

Yüklemek için aşağıdakilere ihtiyacınız olacak:

1. VSCode (version 1.43.0 and above) - [buradan indirebilirsiniz](https://code.visualstudio.com/download); eğer zaten indirdiyseniz burayı görmezden gelin
2. Move IDE - VSCode yüklendikten sonra, [buradan indirebilirsiniz](https://marketplace.visualstudio.com/items?itemName=damirka.move-ide) IDE'nin en güncel versiyonunu kullandığınızdan emin olun.

### Setup environment

Move IDE proposes a single way of organizing your directory structure. Create a new directory for your project and open it in VSCode. Then setup this directory structure:

```
modules/   - directory for our modules
scripts/   - directory for transaction scripts
out/       - this directory will hold compiled sources
```

Also you'll need to create a file called `.mvconfig.json` which will configure your working environment. This is a sample for `libra`:

```json
{
    "network": "libra",
    "sender": "0x1"
}
```

Alternatively you can use `dfinance` as network:

```json
{
    "network": "dfinance",
    "sender": "0x1"
}
```

> dfinance uses bech32 'wallet1...' addresses, libra uses 16-byte '0x...' addresses. For local runs and experiments 0x1 address is enough - it's simple and short. Though when working with real blockchains in testnet or production environment you'll have to use correct address of the network you've chosen.

## Your very first application with Move

Move IDE allows you to run scripts in a testing environment. Let's see how it works by implementing `gimme_five()` function and running it inside VSCode.

### Create module

Create new file called `hello_world.move` inside `modules/` directory of your project.
```Move
// modules/hello_world.move
address 0x1 {
module HelloWorld {
    public fun gimme_five(): u8 {
        5
    }
}
}
```

> If you decided to use your own address (not `0x1`) - make sure you've changed 0x1 in this file and the one below

### Write script

Then create a script, let's call it `me.move` inside `scripts/` directory:
```Move
// scripts/run_hello.move
script {
    use 0x1::HelloWorld;
    use 0x1::Debug;

    fun main() {
        let five = HelloWorld::gimme_five();

        Debug::print<u8>(&five);
    }
}
```

Then, while keeping your script open follow these steps:
1. Toggle VSCode's command palette by pressing `⌘+Shift+P` (on Mac) or `Ctrl+Shift+P` (on Linux/Windows)
2. Type: `>Move: Run Script` and press enter or click when you see the right option.

Voila! You should see the execution result - log message with '5' printed in debug. If you don't see this window, go through this part again.

Your directory structure should look like this:
```
modules/
  hello_world.move
scripts/
  run_hello.move
out/
.mvconfig.json
```

> You can have as many modules as you want in your modules directory; all of them will be accessible in your scripts under address which you've specified in .mvconfig.json
