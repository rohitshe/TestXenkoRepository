# SpriteFont

the @'SiliconStudio.Xenko.Graphics.SpriteFont' class offers an convenient way to draw text. It works with the @'SiliconStudio.Xenko.Graphics.SpriteBatch' class.

# Load a SpriteFont

Since the font is an asset, the user can easily load it. The asset loader will return a @'SiliconStudio.Xenko.Graphics.SpriteFont' object. It contains all the options to display a text (bitmaps, kerning, line spacing etc.).

**Code:** Load a SpriteFont

```cs
var myFont = Asset.Load<SpriteFont>("MyFont");
```


# Write text on screen

Once the font is loaded, the user can display any text on screen with a @'SiliconStudio.Xenko.Graphics.SpriteBatch' object. To learn more about the SpriteBatch, read the [related documentation page](spritebatch.md). The @'SiliconStudio.Xenko.Graphics.SpriteBatch.DrawString' method performs the draw.

**Code:** Write text

```cs
// create the SpriteBatch
var spriteBatch = new SpriteBatch(GraphicsDevice);

// do not forget the begin
spriteBatch.Begin();
 
// draw the text "Helloworld!" in red from the center of the screen
spriteBatch.DrawString(myFont, "Helloworld!", new Vector2(0.5,0.5), Color.Red);
 
// do not forget the end
spriteBatch.End();
```


There a many draw methods. The user can specify the orientation of the text, its scale, its depth, its origin etc. There are also some effects that can be applied on the text through some DrawString methods. They are available as @'SiliconStudio.Xenko.Graphics.SpriteEffects' enum:

- None
- FlipHorizontally
- FlipVertically
- FlipBoth

**Code:** Advanced draw string

```cs
// draw the text "Helloworld!" upside down in red from the center of the screen
spriteBatch.DrawString(myFont, "Helloworld!", new Vector2(0.5,0.5), Color.Red, 0, new Vector2(0,0), new Vector2(1,1), SpriteEffects.FlipVertically, 0);
```


