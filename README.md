fluid-react
===========

[![Maven Central](https://img.shields.io/maven-central/v/io.fluidsonic.react/fluid-react?label=Maven%20Central)](https://search.maven.org/artifact/io.fluidsonic.react/fluid-react)
[![JCenter](https://img.shields.io/bintray/v/fluidsonic/kotlin/react?label=JCenter)](https://bintray.com/fluidsonic/kotlin/react)
[![Kotlin](https://img.shields.io/badge/Kotlin-1.4.20%20(JS)-blue.svg)](https://github.com/JetBrains/kotlin/releases/v1.4.20)
[![React](https://img.shields.io/badge/React-17.0.1-blue.svg)](https://github.com/facebook/react/releases/tag/v17.0.0)
[![#fluid-libraries Slack Channel](https://img.shields.io/badge/slack-%23fluid--libraries-543951.svg?label=Slack)](https://kotlinlang.slack.com/messages/C7UDFSVT2/)

Kotlin/JS wrapper for [React](https://reactjs.org/).

- Similar to [kotlin-react](https://github.com/JetBrains/kotlin-wrappers/tree/master/kotlin-react).
- Nicer and consistent API. Easier to use.
- Not multiplatform. Optimized for Kotlin/JS instead.
- No dependencies beside React.
- Lower size and performance overhead.
- More type safety, esp. around hooks.
- Props allow `class` instead of just `external interface`.
- Hook dependencies use `equals()` instead of `===`, don't require and `Array` and don't have be same count for each render.
- `@DslMarker` colors.
- Router routes are `exact`, `strict` and `sensitive` by default.
- **Highly experimental.** IR compiler only. Relies on unofficial compiler behavior.
- **Work in progress. Please contribute 😃**
- [Kotlin/JS-optimized CSS library](https://github.com/fluidsonic/fluid-css) with nice API in the works. 


## Installation

`build.gradle.kts`:
```kt
dependencies {
    implementation("io.fluidsonic.react:fluid-react-dom:0.9.0")
    implementation("io.fluidsonic.react:fluid-react-router-dom:0.9.0") // if you need routing
}
```


## Example

```kt
import io.fluidsonic.react.*
import kotlinx.browser.*

fun main() {
    val body = checkNotNull(document.body)
    val container = document.createElement("div").also(body::appendChild)

    react.render(container) {
        +"Hello world"

        EmojiContainer(EmojiContainerProps("😍")) { strong { +"cool" } }
    }
}

val EmojiContainer = react.componentWithChildren { props: EmojiContainerProps, children ->
    var count by useState(3)

    useEffect(count) {
        window.setTimeout({ count += 1 }, 2000)
    }

    h1 { +"Your emoji, $count times 🎉" }
    button {
        attrs.onClick = { count += 1 }
        +"Add one"
    }
    ol {
        repeat(count) {
            li {
                +props.emoji
                +" "
                children()
            }
        }
    }
}

class EmojiContainerProps(val emoji: String)
```