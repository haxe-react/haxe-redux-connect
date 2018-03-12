# Haxe Redux Connect

A shorthand for react-redux `connect(...)` to transform a `ReactComponent` into a self-wrapping Higher-Order component.

This is a library meant to work with `haxe-react` and `haxe-redux`, adding support for `@:connect` meta on react components, as the `@connect` decorator of react-redux.
Note that typing is not checked here; however react-redux does some checks at runtime.

`ReactRedux` extern is temporarily included, but will be removed once included in `haxe-redux`.

## Usage

### Installation

Using haxelib:
`haxelib install redux-connect`

### `@:connect` without params

If you do not provide params to the connect meta, a macro will loop through your fields to find *static* fields named `mapStateToProps`, `mapDispatchToProps`, `mergeProps` or `options` and use those it found.

```haxe
@:connect
class MyComponent extends ReactComponentOfProps<Props>
{
	static function mapDispatchToProps(dispatch:Dispatch):Props
	{
		return {
			onClick: function() return dispatch(Actions.Click)
		};
	}

	override public function render()
	{
		return jsx('
			<button onClick=${props.onClick} />
		');
	}
}
```

### Providing params

You can use any function instead of your own static fields.

```haxe
@:connect(null, Helpers.myMapDispatchToProps)
class MyComponent extends ReactComponentOfProps<Props> {}
```

Or even declare your function inline, if you're into this:

```haxe
@:connect(null, function(dispatch:Dispatch) return {onClick: function() return dispatch(Actions.Click))
class MyComponent extends ReactComponentOfProps<Props> {}
```

Please note that you must respect the order of arguments of `ReactRedux.connect()`, so you must pass null as first argument if you only want to define `mapDispatchToProps`, for example.

