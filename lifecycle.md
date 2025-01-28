# Component Lifecycle
|Phase |	Method |	Summary |
|------|----------|----------|
|Creation|	constructor |	Standard JavaScript class constructor . Runs when Angular instantiates the component. |
|Change Detection |ngOnInit	|Runs once after Angular has initialized all the component's inputs.|
||ngOnChanges|	Runs every time the component's inputs have changed.|
||ngDoCheck	|Runs every time this component is checked for changes.|
||ngAfterContentInit |	Runs once after the component's content has been initialized.|
||ngAfterContentChecked |	Runs every time this component content has been checked for changes.|
||ngAfterViewInit |	Runs once after the component's view has been initialized.|
||ngAfterViewChecked |	Runs every time the component's view has been checked for changes. |
|Rendering	| afterNextRender |	Runs once the next time that all components have been rendered to the DOM.|
| |afterRender	| Runs every time all components have been rendered to the DOM.|
| Destruction|	ngOnDestroy	| Runs once before the component is destroyed. |
