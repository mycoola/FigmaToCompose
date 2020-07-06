# Designs to Jetpack Compose converter
Easily convert [Figma](https://www.figma.com/) designs directly to [Jetpack Compose](https://developer.android.com/jetpack/compose) code. (Not affiliated with either of them)

This project contains a plugin that sends the selected Figma json to locahost:9020, and a kotlin backend that converts this json
to Jetpack Compose and sets the clipboard to it

Jetpack Compose: Dev14
Figma release: June 25th 2020

# Usage:
With the [plugin](todo:) installed, start the server by cloning this repo, cd-ing into it and running:

`./gradlew run --args="-config=application.conf"`


The outputted code makes usage of some utility functions, so you'll need this pasted into a file in your project:
(this next code snippet is CC0 / all of my rights waived)
```kotlin
fun spacingArrangement(spacingPx: Int): Arrangement.Vertical = object : Arrangement.Vertical {

    override fun arrange(
        totalSize: Int,
        size: List<Int>,
        layoutDirection: LayoutDirection
    ): List<Int> {
        val positions = mutableListOf<Int>()
        var current = 0
        size.forEach {
            positions.add(current)
            current += it + spacingPx
        }
        return positions
    }
}

@Composable
fun Dp.toIntInPx(): Int = with(DensityAmbient.current) {
    return@toIntInPx this@toIntInPx.toIntPx()
}
```

This requires a JDK installed, if you're doing Android dev it probably already is :)

Now you can open the plugin window in Figma, select a node, and click "Genarate" to get the Jetpack Compose code to display it!

# Use cases
Often times designs contain non-repeated distances, colours and proportions. Replicating these designs can be very tedious, and discrepancies in design and implementation can lead to "split realities".

Primarily, this is to cut out taking measurements, reduce design-implementation back and fourth and save time/energy even when the designer and implementer are the same person

# Features / mappings

|    feature    | Jetpack Compose feature               | Note                                                                                              | Missing / to-do                                    |   |
|:-------------:|---------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------------------|---|
| Frame         | Constraint Layout                     | Supports start, end, scale, stretch, start-end (maintain margins dp), center                      | auto remove redundant constraints                  |   |
| Nested nodes  | Composables with nested calls to them | This is a nice solution for updating implementation to design                                     |                                                    |   |
| Group         | Box                                   |                                                                                                   |                                                    |   |
| Text          | Text                                  | Supports solid colours, font size vertical and horizontal text align                              | Font family, bold, italic, advanced Figma features |   |
| Auto layout   | Row/Column                            |                                                                                                   |                                                    |   |
| Vectors       | Image(vectorResource(...))            | Creates a vector painter looking at a drawable with the svg export name on nodes with svg exports | Automation for importing svgs from figma           |   |
| Rectangle     | Box                                   | Includes generic "style mods" (bg, shadow, )                                                      |                                                    |   |
| Shadow fill   | drawShadow                            | Does not work well for non-rectangles or where shadow is applied to parent of many children       | Other shapes, find solution for shadows on parents |   |
| Corner Radius | .clip(CornerRadiusShape)              |                                                                                                   |                                                    |   |
| Gradient fill | drawBackground(HorizontalGradient(...)|  Only currently supports horizontal gradients                                                     |                                                    |   |

# Development

The backend server accepts Figma's json on a post to `/` on port 9020. Port configurable from application.conf

## Figma plugin development
###### For Linux

 Unfortunately for now a shared clipboard from a Windows or Mac VM or a host running [Barrier](https://github.com/debauchee/barrier) is needed to "conveniently" use local changes,
 until Figma supports developing / running unpublished plugins on Linux

###### Mac or Windows
Within the FigmaPlugin directory lies the code to a plugin that can be imported on Windows or Mac. In Figma client app (not web), go to
Plugins -> Development > New Plugin and select the manifest.json

# TODO:
See Issues
