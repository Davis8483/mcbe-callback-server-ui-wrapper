A wrapper for the `@minecraft/server-ui` Bedrock Edition api which adds callbacks to all components, making dynamic ui's more manageable. Please feel free to use this, no credit needed!

Currently updated to work with version 1.3.0 of `@minecraft/server-ui`

This is used in my [Shovel Land Claim](https://github.com/Davis8483/MCBE-Shovel-Land-Claim) addon, check it out!

## Usage

This wrapper works just like the normal `@minecraft/server-ui` api but with an optional callback!

Here's an example with the `CallbackActionFormData` class

```typescript
const form = new CallbackActionFormData(() => this.main())
    .title({"text": "my action form"})
    .body({
        "rawtext": [
            { "text": "click the button to run a callback" },
            { "text": "\n\n" },
            { "text": "O_O ...so? what are you waiting for!" }
        ]
    })
    .button({"text": "plsss click me!!!"}, "textures/ui/infobulb.png", () => {
        world.sendMessage("ouch that hurt, don't click me so hard next time");
    })
    .button({"text": "i don't have a callback but i'm still quirky"}, "textures/ui/infobulb.png")

form.show(this.player);
```

Buttons are cool and all, but lets spice things up with the `CallbackModalFormData` class

```typescript
const form = new CallbackModalFormData(() => this.main())
    .title({"text": "my modal form"})
    .textField({"text": "enter a number between 1 and 10"}, {"text": "ex: 3"}, undefined, (value) => {
        
        if ((value as String).length == 0){
            return new ModalDataError("§cmust provide a number§r")
        }
        else if ((value as number) < 1) {
            return new ModalDataError("§cmust be above one§r")
        }
        else if ((value as number) > 10) {
            return new ModalDataError("§cmust be below ten§r")
        }

        return new ModalDataCorrect();
    })
    .toggle({"text": "i don't like being on"}, false, (value)=>{
        if (value) {
            return new ModalDataError("§cNO!§r")
        }

        return new ModalDataCorrect();
    })
    .submitButton({"text": "submit"}, (response) => {
        world.sendMessage("You can still use the normal submitButton callback and response variables!\nSwitch value: " + response.formValues[1])
    });

form.show(this.player);
```

Here, you may have noticed the usage of ModalDataError and ModalDataCorrect. These are required returns for all callbacks within the `CallbackModalFormData` class. ModalDataError will show your message inline with the field that was filled out incorrectly.

![image](https://github.com/user-attachments/assets/7308f03c-2a9f-4c58-8962-daa8639ea134)

Btw, here for demonstration purposes all text has been hardcoded although in a real addon it's highly recomended to use [translation keys](https://wiki.bedrock.dev/concepts/text-and-translations).
