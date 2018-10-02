---
title: TextHighlight Directive - Native Angular | Ignite UI for Angular
_description: The Ignite UI for Angular TextHighlight directive can be used to highlight parts of text and have an active highlight on one of them.
_keywords: Ignite UI for Angular, UI controls, Angular widgets, web widgets, UI widgets, Angular, Native Angular Components Suite, Native Angular Components, Native Angular Controls, Native Angular Components Library, Angular TextHighlight Directive, IgxTextHighlight Directive
---

## TextHighlight Directive

The `IgxTextHighlight` directive in Ignite UI for Angular is used to highlight parts of a text, providing options for case sensitive searches and to highlight only exact matches. It also allows the developer to keep an active highlight, which can be any of the already highlighted parts.

### TextHighlight Demo
<div class="sample-container loading" style="height: 260px;">
    <iframe id="text-highlight-1-iframe" frameborder="0" seamless width="100%" height="100%" src="{environment:demosBaseUrl}/text-highlight-1" onload="onSampleIframeContentLoaded(this);"></iframe>
</div>
<div>
    <button data-localize="stackblitz" disabled class="stackblitz-btn" data-iframe-id="text-highlight-1-iframe" data-demos-base-url="{environment:demosBaseUrl}">view on stackblitz</button>
</div>
<div class="divider--half"></div>

> [!NOTE]
> To start using Ignite UI for Angular components in your own projects, make sure you have configured all necessary dependencies and have performed the proper setup of your project. You can learn how to do this in the [**installation**](https://www.infragistics.com/products/ignite-ui-angular/getting-started#installation) topic.

### Usage

To get started with the Ignite UI for Angular TextHighlight directive, let's first import the **IgxTextHighlightModule** in the **app.module.ts** file along with the other Ignite UI for Angular modules we need for our application.

```typescript
// app.module.ts

...
import { IgxButtonModule, IgxInputGroupModule,
        IgxIconModule, IgxRippleModule, IgxTextHighlightModule } from 'igniteui-angular';

@NgModule({
    ...
    imports: [..., IgxButtonModule, IgxInputGroupModule,
                    IgxIconModule, IgxRippleModule, IgxTextHighlightModule],
    ...
})
export class AppModule {}
```

Then, lets create a search box which we can use to highlight different parts of the text. We will use Ignite UI for Angular's [InputGroup](https://www.infragistics.com/products/ignite-ui-angular/angular/components/input_group.html) component in which we will add a text input with buttons for clear matches, find next and find previous and a button for specifying whether the search will be case sensitive or not. Also it has a label for how many matches we have found.

```html
<div class="search-container">
    <igx-input-group type="search" class="input-group">
        <igx-prefix>
            <igx-icon *ngIf="searchText.length == 0">search</igx-icon>
            <igx-icon *ngIf="searchText.length > 0" (click)="clearSearch()">clear</igx-icon>
        </igx-prefix>

        <input #search1 id="search1" igxInput placeholder="Search" [(ngModel)]="searchText" (ngModelChange)="onTextboxChange()"
                (keydown)="searchKeyDown($event)" />
        <igx-suffix>
            <div class="caseSensitiveButton">
                <button igxButton="icon" igxRipple igxRippleCentered="true" (click)="updateSearch()"
                        [igxButtonBackground]="caseSensitive? 'rgb(73, 180, 254)' : 'transparent'">
                    <igx-icon class="caseSensitiveIcon" fontSet="material" name="text_fields"></igx-icon>
                </button>
            </div>
        </igx-suffix>

        <igx-suffix *ngIf="searchText.length > 0">
            <div>
                <span *ngIf="matchCount > 0">
                    {{ index + 1 }} of {{ matchCount }} results
                </span>
                <span *ngIf="matchCount == 0">
                    No results
                </span>
            </div>
            <div class="searchButtons">
                <button igxButton="icon" igxRipple igxRippleCentered="true" (click)="findPrev()" [disabled]="!canMoveHighlight">
                    <igx-icon fontSet="material" name="navigate_before"></igx-icon>
                </button>
                <button igxButton="icon" igxRipple igxRippleCentered="true" (click)="findNext()" [disabled]="!canMoveHighlight">
                    <igx-icon fontSet="material" name="navigate_next"></igx-icon>
                </button>
            </div>
        </igx-suffix>
    </igx-input-group>
</div>
```
Then, we will add a paragraph with text and the IgxTextHighlight directive. Note that, since we need to bind the value input to the text in the paragraph, we will also use interpolation for the paragraph's text. The column, row and page inputs are useful when you have multiple containers and can be left at 0 for our example. Another noteworthy thing is that the search container (in our case the paragraph element) needs to be the only child in its parent container and because of this we need the surrounding div element.

```html
<div>
    <p igxTextHighlight
        [value]="html"
        [groupName]="'group1'"
        [column]="0"
        [row]="0"
        [page]="0"
        [containerClass]="'search-text'"
        class="search-text">
        {{html}}
    </p>
</div>
```

In the .ts file of our component first we need to add the following fields, that are used for bindings in our component's template:

``` typescript
    public html = `
    Use the search box to search for a certain string in this text.
    All the results will be highlighted in yellow, while the first occurrence of the string will be in orange.
    You can use the button in the searchbox to specify if the search will be case sensitive.
    You can move the orange highlight by either pressing the buttons on the searchbox or by using the Enter or the arrow keys on your keyboard.
    `;

    @ViewChild(IgxTextHighlightDirective, {read: IgxTextHighlightDirective})
    public highlight: IgxTextHighlightDirective;

    public searchText: string = "";
    public matchCount: number = 0;
    public caseSensitive: boolean = false;
    public index: number = 0;


    public get canMoveHighlight() {
        return this.matchCount > 1;
    }
```

Then we need to add the following methods which will allow the user to apply the highlights for the text they have typed in the search box and to move the active highlight around.

``` typescript
     public searchKeyDown(ev) {
        if (this.searchText) {
            if (ev.key === "Enter" || ev.key === "ArrowDown" || ev.key === "ArrowRight") {
                ev.preventDefault();
                this.findNext();
            } else if (ev.key === "ArrowUp" || ev.key === "ArrowLeft") {
                ev.preventDefault();
                this.findPrev();
            }
        }
    }

    public onTextboxChange() {
        this.index = 0;
        this.find(0);
    }

    public updateSearch() {
        this.caseSensitive = !this.caseSensitive;
        this.find(0);
    }

    public clearSearch() {
        this.searchText = "";
        this.find(0);
    }

    private findNext() {
        this.find(1);
    }

    private findPrev() {
        this.find(-1);
    }

    private find(increment: number) {
        if (this.searchText) {
            this.matchCount = this.highlight.highlight(this.searchText, this.caseSensitive);
            this.index += increment;

            this.index = this.index < 0 ? this.matchCount - 1 : this.index;
            this.index = this.index > this.matchCount - 1 ? 0 : this.index;

            if (this.matchCount) {
                IgxTextHighlightDirective.setActiveHighlight("group1", {
                    columnIndex: 0,
                    index: this.index,
                    page: 0,
                    rowIndex: 0
                });
            }
        } else {
            this.highlight.clearHighlight();
        }
    }
```

If the sample is configured properly, the final result should look like that:

<div class="sample-container loading" style="height: 260px;">
    <iframe id="text-highlight-1-iframe" src='{environment:demosBaseUrl}/text-highlight-1' width="100%" height="100%" seamless
        frameBorder="0" onload="onSampleIframeContentLoaded(this);"></iframe>
</div>
<div>
<button data-localize="stackblitz" disabled class="stackblitz-btn" data-iframe-id="text-highlight-1-iframe"
    data-demos-base-url="{environment:demosBaseUrl}">view on stackblitz</button>
</div>

<div class="divider"></div>

### Search across multiple containers
The igxTextHighlight allows you to search across multiple containers which all share one active highlight. This is done by having the same group value across multiple TextHighlight directives which have all separate containers. In order to setup the sample we will reuse the search box from the previous sample, but this time we will add two paragraphs. Again, note that they both are in their own containers, but this time the second one has a different row value.

```html
    <div>
        <p igxTextHighlight
            [groupName]="'group1'"
            [column]="0"
            [row]="0"
            [page]="0"
            [containerClass]="'search-text'"
            [value]="firstParagraph"
            class="search-text">
            {{firstParagraph}}
        </p>
    </div>
    <div>
        <p igxTextHighlight
            [groupName]="'group1'"
            [column]="0"
            [row]="1"
            [page]="0"
            [containerClass]="'search-text'"
            [value]="secondParagraph"
            class="search-text">
            {{secondParagraph}}
        </p>
    </div>

```
Then in the .ts file we have the firstParagraph and secondParagraph fields, which are bound to the respective value inputs of the text highlight directives. Also we will now use ViewChildren instead of ViewChild to get all the highlights in our template.

```typescript
    public firstParagraph = `
        Use the search box to search for a certain string in the paragraph below.
        All the results will be highlighted in yellow, while the first occurrence of the string will be in orange.
        You can use the button in the searchbox to specify if the search will be case sensitive.
        You can move the orange highlight by either pressing the buttons on the searchbox or by using the Enter or the arrow keys on your keyboard.
`;

    public secondParagraph = `
On top of the functionality from the previous sample, this sample demonstrates how to implement the text highlight directive
             with several different containers. In this case, we have two paragraphs, each containing some text. You can see that
             they share the same active (orange) highlight and the returned match count includes both containers. The find method in this
             sample can be reused regardless of the number of containers you have in your application.
    `;

    @ViewChildren(IgxTextHighlightDirective)
    public highlights;
```
All the rest of the code in the .ts file is identical to the single container example with the exception of the find method. Changes to this method are necessary since we now have multiple containers, but the code there can be used regardless of the number of TextHighlight directives you have on your page.

```typescript
    private find(increment: number) {
        if (this.searchText) {
            let count = 0;
            const matchesArray = [];

            this.highlights.forEach((h) => {
                count += h.highlight(this.searchText, this.caseSensitive);
                matchesArray.push(count);
            });

            this.matchCount = count;

            this.index += increment;
            this.index = this.index < 0 ? this.matchCount - 1 : this.index;
            this.index = this.index > this.matchCount - 1 ? 0 : this.index;

            if (this.matchCount) {
                let row;

                for (let i = 0; i < matchesArray.length; i++) {
                    if (this.index < matchesArray[i]) {
                        row = i;
                        break;
                    }
                }

                const actualIndex = row === 0 ? this.index : this.index - matchesArray[row - 1];

                IgxTextHighlightDirective.setActiveHighlight("group1", {
                    columnIndex: 0,
                    index: actualIndex,
                    page: 0,
                    rowIndex: row
                });
            }
        } else {
            this.highlights.forEach((h) => {
                h.clearHighlight();
            });
            this.matchCount = 0;
        }
    }
```

<div class="sample-container loading" style="height: 400px;">
    <iframe id="text-highlight-2-iframe" frameborder="0" seamless width="100%" height="100%" src="{environment:demosBaseUrl}/text-highlight-2" onload="onSampleIframeContentLoaded(this);"></iframe>
</div>
<div>
    <button data-localize="stackblitz" disabled class="stackblitz-btn" data-iframe-id="text-highlight-2-iframe" data-demos-base-url="{environment:demosBaseUrl}">view on stackblitz</button>
</div>

<div class="divider"></div>

### API Summary

The following tables summarize some of the useful TextHighlight directive inputs and methods.

#### Inputs

The following inputs are available in the **igxTextHighlight** directive:

| Name | Type | Description |
| :--- | :--- | :--- |
| `groupName` | string | Defines the group in which the active highlight will be shared. |
| `row` | number | Defines the row on which the directive is positioned. |
| `column` | number | Defines the column on which the directive is positioned. |
| `page` | number | Defines the page on which the directive is positioned. |
| `value` | any | Defines the underlying value on which we will perform searches. |

<div class="divider"></div>

#### Methods

The following methods are available in the **igxTextHighlight** directive.

| Name | Return type | Parameters | Description |
| :--- | :--- | :--- | :--- |
| `highlight` | number | The text to highlight and optionally whether the search is case sensitive and should we highlight only exact matches | Highlights the searched string (if it exists in the value input) and returns how many times the value contains the searched highlighted string|
| `clearHighlight` | void | none | Clears all existing highlights |
| `activateIfNecessary` | void | none | Activates a highlight if it is the current active highlight |
| `setActiveHighlight` | void | The groupName and an object implementing an interface with the row, column and page to activate | Activates the highlight from the specified group on the given row, column and page |
| `clearActiveHighlight` | void | The group name | Clears the active highlight from the specified group |

<div class="divider"></div>

### Additional Resources
* [Grid Search](https://www.infragistics.com/products/ignite-ui-angular/angular/components/grid_search.html)

<div class="divider--half"></div>
Our community is active and always welcoming to new ideas.

* [Ignite UI for Angular **Forums**](https://www.infragistics.com/community/forums/f/ignite-ui-for-angular)
* [Ignite UI for Angular **GitHub**](https://github.com/IgniteUI/igniteui-angular)