# SpriteFont

the @'SiliconStudio.Xenko.Graphics.SpriteFont' class offers an convenient way to draw text. It works with the @'SiliconStudio.Xenko.Graphics.SpriteBatch' class.

# Loading a SpriteFont

After a font asset is compiled it can be loaded as a @'SiliconStudio.Xenko.Graphics.SpriteFont' instance using the @'SiliconStudio.Core.Serialization.Assets.ContentManager'. It contains all the options to display a text (bitmaps, kerning, line spacing etc.).

**Code:** Load a SpriteFont

```cs
var myFont = Content.Load<SpriteFont>("MyFont");
```


# Writing text on screen

Once the font is loaded, the user can display any text on screen with a @'SiliconStudio.Xenko.Graphics.SpriteBatch'. To learn more about the SpriteBatch, read the [related documentation page](spritebatch.md). The @'SiliconStudio.Xenko.Graphics.SpriteBatch.DrawString' method performs the draw.

**Code:** Write text

```cs
// create the SpriteBatch
var spriteBatch = new SpriteBatch(GraphicsDevice);

// do not forget the begin
spriteBatch.Begin(GraphicsContext);
 
// draw the text "Helloworld!" in red from the center of the screen
spriteBatch.DrawString(myFont, "Helloworld!", new Vector2(0.5, 0.5), Color.Red);
 
// do not forget the end
spriteBatch.End();
```


The various overloads allow to specify the text's orientation, scale, depth, origin, etc. There are also some @'SiliconStudio.Xenko.Graphics.SpriteEffects' that can be applied to the text:

- None
- FlipHorizontally
- FlipVertically
- FlipBoth

**Code:** Advanced text drawing

```cs
// draw the text "Helloworld!" upside down in red from the center of the screen
spriteBatch.DrawString(myFont, "Helloworld!", new Vector2(0.5, 0.5), Color.Red, 0, new Vector2(0, 0), new Vector2(1,1), SpriteEffects.FlipVertically, 0);
```


