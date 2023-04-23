# Atramentum Luminis Authoring Manual

⚠️ A word of warning before beginning: some of the manipulations described in the later sections of this manual _can crash your game if done wrong_. Please carefully read and fully understand any paragraph marked with warning signs before proceeding. ⚠️

Please note that this manual expects the reader to have a certain level of knowledge about general texture editing, and knowledge about how to locate the wanted files in Penumbra.

It also assumes that the reader can use a texture editing program that fully preserves color information even in transparent pixels.

If you make mods that target Atramentum Luminis and/or write alternate versions or complements of this manual or parts of it, please make sure to specify the frameworks they are for, including version: Atramentum Luminis major version number changes if and only if some compatibility with previous versions is broken (in other words, 3.x is somewhat incompatible with 2.x, and 2.x is somewhat incompatible with 1.x – though it will take 271 more compatibility breaks to reach version 274.0 😉).

## Level 1: Textures-only

This level only requires editing textures to put information in some channels that are unused by the vanilla game.

The mods you can make will target the "Textures-only (Legacy)" skin and iris frameworks.

### Iris: Glow (Emissive)

To add glow information to irises, you have to edit the multi map (which has a filename usually ending in `_s.tex`).

The glow information is to be added to the multi map's alpha channel, in inverted form, that is:
- Where the alpha channel is "white" (so the texel is opaque), things will behave as in the vanilla game, with no glow ;
- Where the alpha channel is "black" (so the texel is transparent), the diffuse will be wholly turned into emissive, in other words the eye will glow at maximum intensity ;
- In-between alpha values can be used to locally adjust the intensity of the effect.

### Skin: Glow (Emissive)

To add glow information to skin (including nails, sclerae, Auri scales), you have to edit the diffuse map (which has a filename usually ending in `_d.tex`).

The glow information is to be added to the diffuse map's alpha channel, in inverted form (see above).

### Compatibility

The vanilla game ignores the aforementioned alpha channels. Therefore, using textures with edited alpha without Atramentum Luminis won't cause bugs or visual glitches, it will just give the same result as if the alpha edit was not done.

## Level 2: Textures and Materials

This level requires editing textures the same way as above, but also materials.

The mods you can make will target the "Textures and Materials" skin and iris frameworks.

Please note that, while locating materials can be tedious in the current stable Penumbra version (0.6.6.5 at the time of writing), work is ongoing to eventually make it easier.

For all of the sections below, you first have to select the wanted material in Penumbra's Advanced Editing window and go into the `g_MaterialParameter` part, in Advanced Shader Resources. Once you're done, don't forget to save your file.

### Iris/Skin: Glow Adjustment

To globally adjust the iris or skin glow, add the constant with ID `0x8F6498D1` (it may already be there if editing an already modded material).

- The first value is the global glow multiplier in well-lit environments ;
- The second value is the global glow multiplier in dark environments.

Both values must be set to 1 if you just want to have the same result as with the "Textures-only (Legacy)" frameworks.

### Skin: Auri Scale Iridescence

To add an iridescence effect to Auri scales like in the Hannish scale ointment mod, add the constant with ID `0x4103FEEF` to a skin material.

Auri scales are detected depending on the red channel of the multi map: by default, values between 25% and 75% are considered scales.

- The first value is the global iridescence effect multiplier ;
- The second value shifts the scale detection range: for example (assuming other control values are 0), 0 means 25% to 75%, -0.2 means 5% to 55% ;
- The third value narrows or expands the scale detection range: for example (assuming other control values are 0), 0 means 25% to 75%, 0.1 means 15% to 85% ;
- The fourth value sharpens or softens the ends of the scale detection range: for example (assuming other control values are 0), 0 means that the effect gradually rolls off from 30% to 20%, -0.05 means that it's all-or-nothing above and below 25% ;
- The fifth value attenuates the effect by applying a bias to the normals: 0 is equivalent to HSO's "Vibrant" options, 0.5 is equivalent to HSO's "Normal" options, 1 is equivalent to HSO's "Faint" options ;
- The sixth value is the maximum chroma radius to use: 0 will make the effect gray, 50 will be equivalent to HSO ;
- The seventh value is the hue angle shift in degrees: 0 gives a reddish hue to scales that face towards the right of the screen, positive values rotate the effect counterclockwise ;
- The eight value is the hue angle multiplier (it will be rounded to the nearest integer).

The effect is compatible with most scale overlays with the three control values left at 0, and should be compatible with all of them by tweaking these values.

Note: all the builds of Hannish scale ointment since 2.0 are actually just special builds of Atramentum Luminis's "Textures-only (Legacy)" skin framework, with this constant forced to 1, 0, 0, 0, 0|0.5|1, 50, 0|90|180|-90, 1|-1|3.

### Skin: Asymmetry Adapter

If you want to use an asymmetric texture with a symmetric model, or a symmetric texture with an asymmetric model, add the constant with ID `0x5E3ABDFB`.

- If your texture matches your model, the value must be set to 0 ;
- If your texture is asymmetric and your model is symmetric, the value must be set to 1 ;
- If your texture is symmetric and your model is asymmetric, the value must be set to -1.

Please note that the remapping functions used are very simple (namely, `uAsym = (1±uSym)/2`, `tAsym = ±tSym`, u and t denoting respectively the horizontal texture coordinate and the surface tangent vector). It happens that the Bibo+ and The Body SE UV layouts and surface geometries have a relationship with the vanilla/Gen2 ones that's mostly accurately modeled by these equations, but it's not perfect (especially for NSFW). If you want an asymmetric texture that completely works on vanilla/Gen2 models, you'll have to work with a properly "unfolded" vanilla/Gen2 texture.

Also, due to Gen3 not having a straightforward enough relationship with vanilla/Gen2, the asymmetry adapter will likely never work with it (contributions are welcome though 😉).

The left half of the textures will be applied to the right half of the body/face/tail/…, and the right half of the textures will be applied to the left half of the body/face/tail/… (if you set the camera right in front of your character, the left of the texture will be to the left of the screen).

*TL;DR*: The asymmetry adapter "unfolds" a symmetric UV layout. It happens to mostly work with Bibo+ and The Body SE because they're similar to "unfolded" vanilla/Gen2. It doesn't work at all with Gen3 because it's not.

### Iris: Asymmetry Adapter

If you want to use an asymmetric texture with a symmetric model, a symmetric texture with an asymmetric model, or an asymmetric catchlight map, add the constant with ID `0x5E3ABDFB`.

- The first value works in the same way as the value for the skin asymmetry adapter ;
- The second value must be 0 if your catchlight map is symmetric, 1 if it's asymmetric.

The left half of the textures will be applied to the right eye, and the right half of the textures will be applied to the left eye (if you set the camera right in front of your character, the left of the texture will be to the left of the screen).

Please note that the catchlight map's UVs have nothing to do with the vertex UVs: instead, they are calculated using the normal vector. It is therefore impossible to change the catchlight map's UV layout using something else than a shader package without adverse effects.

### Iris/Skin: 1.x/2.x-like Bloom Effect

The overdone bloom effect that was found in Atramentum Luminis 1.x and 2.x was due to it being calculated using a wrong formula (in some way, it's a bug that's been fixed in 3.0).

Yet, if you liked it, it's still available, and can be configured by adding the constant with ID `0xA5EDBE5C`.

- The first value is the global bloom multiplier in well-lit environments ;
- The second value is the global bloom multiplier in dark environments.

### Compatibility

The vanilla game ignores the new parameters, and the "Textures-only (Legacy)" frameworks take into account some of the parameters and ignore the others (yet, if you do any material edits, it's recommended to work only with the "Textures and Materials" frameworks). Therefore, using edited materials without Atramentum Luminis will give the same result as if the parameters were not defined. Using edited materials with "Textures-only (Legacy)" frameworks will give results that may be stronger or weaker than indended.

## Level 3: More Textures and Materials

```
                  .;=,;===,
                 //\ / _,'
                /"/='-':==-.     HERE
                `'`==.(\    )
              ,' :    '"          BE
             : : :
     ___   .' :: : .    __:.    DRAGONS
    /___\ .'_:\:/:::\: /:.:\.
l42_|++n|:|+::|:|::n:::|.::|:.
```

This level requires editing textures and materials, ⚠️ _in ways that can crash your game if done wrong_ ⚠️.

The mods you can make will target the "Textures and Materials" skin and iris frameworks.

Please note that this level changes how some things are interpreted in such a way that, just as with level 2, only enabling it without proper configuration can nullify some of the effects with no benefit.

### Skin: Switching to level 3

In Samplers (in Advanced Shader Resources), add g_SamplerEmissive and g_SamplerEffectMask.

Set g_SamplerEmissive's sampler flags to the same value as those of g_SamplerDiffuse, and g_SamplerEffectMask's sampler flags to the same value as those of g_SamplerMask (they should be the same anyway).

The texture flags must be set to `8000` if you want the game to add the `--` prefix to the texture file name, otherwise `0000`.

Go back to the top of the editor and set the texture path for g_SamplerEmissive and g_SamplerEffectMask to `chara/common/texture/transparent.tex` (unless you already have the paths to the actual maps you plan to use).

There is no standard path for these new maps, but I suggest following the convention of the existing maps of your material, with a `_e` suffix for the emissive and `_x` for the effect mask.

⚠️ To avoid crashes, all of the above must be done before doing the step below, and the step below must be undone before undoing any of the above. ⚠️

In Shader Keys (still in Advanced Shader Resources), add key `0x9D4A3204` and set it to `0x1E8ABB16`.

### Iris: Switching to level 3

In Samplers, add g_SamplerDiffuse, g_SamplerEmissive and g_SamplerEffectMask.

Set g_SamplerDiffuse, g_SamplerEmissive and g_SamplerEffectMask's sampler flags to the same value as those of g_SamplerMask.

Go back to the top of the editor and set the texture path for g_SamplerDiffuse, g_SamplerEmissive and g_SamplerEffectMask to `chara/common/texture/transparent.tex` (unless you already have the paths to the actual maps you plan to use).

There is no standard path for these new maps, but I suggest following the convention of the existing maps of your material, with a `_d` suffix for the diffuse, `_e` for the emissive and `_x` for the effect mask.

⚠️ To avoid crashes, all of the above must be done before doing the step below, and the step below must be undone before undoing any of the above. ⚠️

In Shader Keys (still in Advanced Shader Resources), add key `0x9D4A3204` and set it to `0x1E8ABB16`.

### Skin: Using the emissive map

The red, green and blue channels of the emissive map are actual color data.

The alpha channel of the emissive map is interpreted as "how much does this map override the emissive that level 2 would have generated".

The glow adjustment parameter still applies to the emissive map.

### Iris: Using the diffuse and emissive maps

The red, green and blue channels of the diffuse and emissive maps are actual color data.

The alpha channel of the diffuse and emissive maps is interpreted as "how much does this map override the diffuse/emissive that level 2 would have generated".

The multi map's alpha channel and glow adjustment parameter still apply to the diffuse map, and the glow adjustment parameter also still applies to the emissive map.

### Iris: Using the effect mask

The red, green and blue channels are currently unused. They should be left at zero (i. e. the texture should be only gradients of black to transparent).

The alpha channel is used to locally control the bloom effect (instead of the inverted multi map's alpha channel in level 2): where the alpha channel is "black" (so the texel is transparent), the bloom effect will be disabled, where the alpha channel is "white" (so the texel is opaque), the bloom effect will be at full power. The glow adjusment and bloom effect parameters still apply.

### Skin: Using the effect mask

The red channel is used to control the iridescence effect (instead of a function of the multi map's red channel as in level 2): where the red channel is black, the iridescence effect will be disabled, where the red channel is red, the iridescence effect will be at full power. Consequently, the scale detection control values (second, third and fourth) of the iridescence parameter are ignored. The other values still apply.

The green channel is used to control the perma-wetness effect (BETA): where the green channel is black, the skin will be normally dry or wet depending on the environment, where the green channel is green, the skin will be permanently wet.

The blue channel is used to control the metallic finish effect (BETA): where the blue channel is black, the effect will be disabled, when the blue channel is blue, the skin will get a metallic finish, which can be used for example for visible implants.

The alpha channel is used to locally control the bloom effect as in the iris effect mask. The glow adjustment and bloom effect parameters still apply.

You can use the following JSON file in PNG Mapper to automatically generate an effect mask with scale iridescence out of a multi, assuming scale detection control values are all zero (otherwise, the script can be tweaked accordingly):

```json
{
  "inputs": [ "in_m" ],
  "outputs": [ "out_x" ],
  "mapping": "out_x.r = 1.0 - smoothstep(\n  0.2, 0.3, Math.abs(in_m.r - 0.5));\n\nout_x.g = 0.0;\nout_x.b = 0.0;\nout_x.a = 1.0;"
}
```

### Compatibility

The vanilla game and the "Textures-only (Legacy)" frameworks ignore the new shader key and samplers. Therefore, using materials with level 3 edits without Atramentum Luminis or with a "Textures-only (Legacy)" framework will give the same results as if the shader key and samplers were not defined.

### Tips

The game provides a few solid color textures at the following paths (figuring out which texture has which color is left as an exercise to the reader 😉):

- `chara/common/texture/transparent.tex` ;
- `chara/common/texture/black.tex` ;
- `chara/common/texture/white.tex` ;
- `chara/common/texture/red.tex` ;
- `chara/common/texture/green.tex` ;
- `chara/common/texture/blue.tex`.

While all of the samplers (and associated textures) are mandatory for a level 3 material, if you don't need one of them or want to set what it controls to a uniform value, you can use these textures instead of making your own.
