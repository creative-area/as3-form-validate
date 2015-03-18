# FormValidate

**FormValidate** is an **ActionScript 3** class.

It's a simple tool to validate and submit a form with simple configuration.


## Example

In Flash IDE :

  * Create two dynamic TextFields and name them "name" and "email" (form input field).
  * Create a dynamic TextField and name it "msg" (display messages : error or success).
  * Drop a button and name it "submit".
  * Write code :

```as3
// import class
import com.creativearea.FormValidate;

// form config
var f_config = {
	errorTextField:'msg',
	fields:{
		name:{
			rules:{
				required:true
			},
			msg:{
				required:"Name is required !"
			}
		},
		email:{
			rules:{
				required: true,
				email: true
			},
			msg:{
				required:"Email is required !",
				email:"Email must be valid !"
			}
		}
	}
};

// declare new form
var f = new com.creativearea.FormValidate(this, f_config);

// validation button
function f_submit(e:MouseEvent){
	f.showMessage('verif form ...');
	if ( f.validate() ) {
		f.showMessage('form ok !');
	} else {
		f.showMessage(f.getErrors().join(', '));
	}
}
this.submit.addEventListener(MouseEvent.CLICK, f_submit);
```


## Documentation

### FormValidate class ###

#### Constructor ####

```as3
FormValidate(rootMovieClip:MovieClip, configParams:Object);
```

`rootMovieClip` : name of the MovieClip that contains the TextFields of the form.

`configParams` : config object (see configuration on next section)

#### Properties ####

```as3
root:MovieClip
```

name of the MovieClip that contains the TextFields of the form.

```as3
config:Object
```

config object (see configuration on next section).

```as3
onSubmitError:Function(errors:Object)
```

assign new callback function on submit error. argument is an object with `type` (String) and `message` (String) properties.

```as3
showMessage:Function(message:String)
```

replace internal `showMessage` method by the function in argument.

#### Methods ####

```as3
validate():Boolean
```

return `true` if the form is valid, otherwise `false`.

```as3
getErrors():Array
```

return indexed array of error messages.

```as3
showMessage(message:String):void
```

message : string to be displayed. by default, it use `trace()` function. if params `errorTextField` is set, it use this textField to render the message.

```as3
submit(callback:Function):void
```

callback : function called on the `Event.COMPLETE` (get result of the posted values)

send form fields values using `POST` method to the `url` param (see config object). if a `cbSend` param is defined for a field, a method with the name of the param will be call before value is sent. this function must get one argument (the value) and must return the transformed (or not) value.

```as3
addValues(values:Object):void
```

`values` : an object that will be added to the `POST` values. these values are added before the send, but after getting values from the form.


### Config Object ###

the configParams is an object like this (all features availables are listed) :

```as3
configParams =
{
	url:"url"
	stopOnError:Boolean,
	// if textfield is not directly in this.root, reassign showMessage function
	errorTextField:"textfield_name",
	fields:{
		field_name:{
			label: "display_name",
			path: "path_to_field",
			rules:{
				required: Boolean,
				email: Boolean,
				minlength: Number,
				maxlength: Number,
				equalto: "field_name",
				date: Boolean,       // dd/mm/yyyy
				serverdate: Boolean, // yyyy-mm-dd
				ofage: Boolean       // age not under 18
			},
			msg:{
				required:"Message"
			},
			// tabulation index
			tabIndex:Number,
			// call a method named "getValue_[field_name]()" to render the value.
			// called internaly before rules validation
			cbGet:Boolean ,
			// call a method named "[method_name]" to trasform the value.
			// called internaly before submit. the return value remplace the get value.
			cbSend:"method_name"
		},
	}
}
```

### Configuration detail ###

Features in config param (key : (TypeOfValue)value) :

  * `fields` : (Object)fields config (see next section)
  * `errorTextField` : (String)Name of the dynamic TextField (display messages)
  * `stopOnError` : (Boolean)Stop validating on first error
  * `url` : (String)Url of web page to POST form fields.

Features in field validation config :

The fields config is an Object : each key is the name of the MovieClip toi validate and the corresponding value is the validation options.

Availables options (key : (TypeOfValue)value) :

  * `label` : (String)Name of the field (used in error messages). If not set, the name of the MovieClip is used.
  * `rules` : collection of key/value. The rules to validate. You can associate each rule with a message (msg).
  * `msg` : collection of key/value. The error messages displayed if corresponding rule is not validated.
  * `cbGet` : (Boolean|String)field name. Call a method named getValue_"field\_name"() to render the value. Called internaly before rules validation.
  * `cbSend` : (String)method name. Call a method named "method name" to transform the value. Called internaly before submit. The return value remplace the get value._

Availables rules (key : (TypeOfValue)value) :

  * required: Boolean,
  * email: Boolean,
  * minlength: Number,
  * maxlength: Number,
  * equalto: "field\_name",
  * date: Boolean, // dd/mm/yyyy
  * serverdate: Boolean, // yyyy-mm-dd
  * ofage: Boolean // age not under 18


---


Thanks [jQuery Validate](http://jqueryvalidation.org/) for the idea and architecture.
