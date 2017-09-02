# \<xtal-composition\>

Create WebCompositions

Shame about losing those HTMLImports. But let's take this opportunity to build something less granular than WebComponents while HTMLImports go down in flames.

I call them WebCompositions, or WebCompost for short.

I've observed that up until now, web component development (particularly with Polymer) has tended to fall into one of two fairly distinct categories:  1)  Crafting stable, resuable components.  These will tend to be specialized, often code-centric components, and not change very often 2) Assembling application specific, content-heavy views. These will tend to be heavy on highly fluid markup content (referred to henceforth as "mulch", combined with declarative configuration settings.

Current day world wide worm browsers are best be able to digest content if is represented as raw HTML / other markup, sprinkled with a little JavaScript pixie dust to hold it together.  In contrast to synthetic markup abstractions contained within JavaScript or Wasm.  Which the Web Component working group is pushing us to do.

So let's think outside the constraints of components, and fashion a variation on components that can play well to the web's strengths even without the w3c's blessings.

\<xtal-ref\> is a key component needed for WebCompositions, especially when the way the markup (or "mulch") will be loaded will be via simple client-side includes pointing to an HTML document. The mulch will actually contain web component tags ("seeds"), which may be configurable from the outside by passing in different properties (to the light children).  \<xtal-ref\> will effectively "water" the web component "seeds."


The need for a trellis

With Polymer 2, even when designing application specific views, the usefulness of the "controlling" class where code could go to fill in the gaps of what could be accomplished declaratively cannot be denied.  Things like responding to one component being clicked, listening for an object in the "scope" for changes, etc.  In the absense of W3C support for a native HTML web component, therefore, we do need some skeleton structure where we can place our code.  The key difference with WebComponents, though, is that this code need not be reusable.

We propose the following work in progress outline of how a web composition document would be layed out:

```html
<!DOCTYPE html>
<xtal-composition id="myPage" customer="[[item.customer]]"> <!-- get passed properties from parent -->
    <light-children>
        <div id="myDiv"> 
            <xtal-ref></xtal-ref> <!-- "Waters" the web component "seeds" -->
            <my-custom-element child-prop="[[customer.LastPurchase]]" on-click="handleClickOnMyCustomElement" upgrade-me>
                Temporary content(?) prior to being upgraded that displays immediately
            </my-custom-element>
            <your-custom-element upgrade-me>
                More temporary content(?) prior to being upgraded that displays immediately
            </your-custom-element>
            <table>
            <tr>
                <td><my-left-hand-component upgrade-me>More light content</my-left-hand-component></td>
                <td><my-right-hand-component upgrade-me>Last bit of light content</my-right-hand-component></td>
            </tr>
        </table>
        </div>
        
    </light-children>
    <shadow-dom>
        <template is="dom-bind">
            <private-component></private-component>
            <slot>
                <!-- light-children contents will go here after all elements are upgraded (maybe?) -->
            </slot>
        </template>
    </shadow-dom>
    <script is="trellis">
        {
            properties:{
                customer:{
                    type: Object,
                    observer: onPropsChange
                }
            }
            handleClickOnMyCustomElement: (e) =>{

            }
            onPropsChange(){

            }
        }
    </script>
</xtal-composition>

```



## Install the Polymer-CLI

First, make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `polymer serve` to serve your element locally.

## Viewing Your Element

```
$ polymer serve
```

## Running Tests

```
$ polymer test
```

Your application is already set up to be tested via [web-component-tester](https://github.com/Polymer/web-component-tester). Run `polymer test` to run your application's test suite locally.
