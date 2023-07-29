# General Information about Materials

This document contains general information about skin, iris and hair materials of a popular game.

## Material parameters

| Key        | Name                | Type       | Defined by         |
|------------|---------------------|------------|--------------------|
| `15B70E35` | ???                 | Real       | Vanilla            |
| `174BB64E` | g_LipFresnelValue0  | RGB        | Vanilla            |
| `29AC0223` | g_AlphaThreshold    | Real       | Vanilla            |
| `2C2A34DD` | g_DiffuseColor      | RGB        | Vanilla            |
| `2E60B071` | g_TileScale         | 2 reals    | Vanilla            |
| `36080AD0` | g_SpecularMask      | Real       | Vanilla            |
| `38A64362` | g_EmissiveColor     | RGB        | Vanilla            |
| `4103FEEF` | g_ScaleIridescence  | 8 reals    | Atramentum Luminis |
| `4255F2F4` | g_TileIndex         | Integer    | Vanilla            |
| `575ABFB2` | g_AreaAlignment ??? | Real       | Vanilla            |
| `5E3ABDFB` | g_AsymmetryAdapter  | Integer(s) | Atramentum Luminis |
| `62E44A4F` | g_FresnelValue0     | RGB        | Vanilla            |
| `878B272C` | g_LipShininess      | Real       | Vanilla            |
| `8F6498D1` | g_EmissiveRedirect  | 2 reals    | Atramentum Luminis |
| `992869AB` | g_Shininess         | Real       | Vanilla            |
| `A5EDBE5C` | g_LegacyBloom       | 2 reals    | Atramentum Luminis |
| `B500BB24` | g_ScatteringLevel   | Real       | Vanilla            |
| `B5545FBB` | g_NormalScale       | Real       | Vanilla            |
| `D367C386` | g_StdHairInfluence  | Real       | Atramentum Luminis |

Material parameters are always encoded as C `float`s. "Integer" means any supplied fractional part will be at best useless, at worst only harmful, and "RGB" means the parameter is a triplet of reals with red-green-blue semantics.

## Material shader keys

| Key        | Default value     | Alternate values                | Defined by         |
|------------|-------------------|---------------------------------|--------------------|
| `380CAED0` | `F5673524` Face   | `2BDB45F1` Body, `57FF3B64` ??? | Vanilla            |
| `9D4A3204` | `698D8B80` Level2 | `1E8ABB16` Level3               | Atramentum Luminis |
| `F52CCF05` | `DFE74BAC` Color  | `A7D2FF60` Mask                 | Vanilla            |
