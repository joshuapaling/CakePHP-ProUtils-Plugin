# ProUtils Plugin for CakePHP 2.1 #

The ProUtils plugin is a growing collection of useful components, behaviors, helpers, and libraries. Here we will list and detail each component.

## Behaviors

* CsvExport        - adds the ability to export csv data from the model.
* EventDispatcher        - adds CakeEvent dispatchers for all common Model callbacks.

### CsvExportBehavior ###

Adds the exportCSV method to the model, exports all records.

You can configure the CsvExport behavior using these options:

* delimiter - The delimiter for the values, default is ;
* enclosure - The enclosure, default is "
* encoding - utf8 or default encoding, default is utf8
* max_execution_time - Extend the execution time for larger datasets, default is 360

The main method of this behavior is
    
    $csvdata = $this->Model->exportCSV();

Enable within your model
    
    public $actsAs = array( 'ProUtils.CsvExport' );

Export data and present as a file download from your controller

    function export() {
        $this->autoRender = false;
        $modelClass = $this->modelClass;
        $this->response->type('Content-Type: text/csv');
        $this->response->download( strtolower( Inflector::pluralize( $modelClass ) ) . '.csv' );
        $this->response->body( $this->$modelClass->exportCSV() );
    }
    
### EventDispatcherBehavior ###

Uses the CakePHP 2.1 Events System to dispatch events for all of the
common Model callbacks.
 
Event listeners that register for an Event will have access to the
$Model instance as the $Event->subject(), and some callbacks also 
present addition arguments dictated by the specific callback. Read
more about CakePHP 2.1's Events System [here](http://book.cakephp.org/2.0/en/core-libraries/events.html)

Config parameters can disable certain events alltogether, or change the
behavior of the Model when an Event is canceled by a listener. 

You can configure the EventDispatcher behavior using these options:

* beforeFind
* afterFind
* beforeSave
* afterSave
* beforeDelete
* afterDelete
* beforeValidate

Each of the above can be configured with three options

* disable - Turn on/off Event dispatching for a specific callback, default false.
* eventName - The event name dispatched, default Model.ModelName.callbackName.
* onPropagationStop - Defines how a stopped event will be handled, default varies, see example below.
** 'continue' - on Event stop, discontinue processing further listeners but does not abort the callback.
** 'abort' - on Event stop, discontinue processing further listeners, and aborts the callback.
** 'die' - on Event stop die()
** 'exception' - on Event stop throw an exception

Example Usage within your model
    
    public $actsAs = array( 'ProUtils.EventDispatcher' );
    
Example Usage with all default configuration values

    public $actsAs = array( 'ProUtils.EventDispatcher'=>array(
        'beforeFind' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.beforeFind',
            'onStopPropagation'=>'continue'
        ),
        'afterFind' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.afterFind',
            'onStopPropagation'=>'continue'
        ),
        'beforeSave' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.beforeSave',
            'onStopPropagation'=>'abort'
        ),
        'afterSave' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.afterSave',
            'onStopPropagation'=>'continue'
        ),
        'beforeDelete' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.beforeDelete',
            'onStopPropagation'=>'abort'
        ),
        'afterDelete' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.afterDelete',
            'onStopPropagation'=>'continue'
        ),
        'beforeValidate' => array(
            'disable'=>false,
            'eventName'=>'Model.{alias}.beforeValidate',
            'onStopPropagation'=>'abort'
        )
    ) );
    
Events Fired by this EventDispatcher behavior          

* Model.ModelName.beforeFind
* Model.ModelName.afterFind
* Model.ModelName.beforeSave
* Model.ModelName.afterSave
* Model.ModelName.beforeDelete
* Model.ModelName.afterDelete
* Model.ModelName.beforeValidate

## Libraries ##

* ProTemplateCompiler       - Compile a document from a template and data array.

## Installation ##

    git clone https://github.com/pronique/CakePHP-ProUtils app/Plugin/ProUtils
    
    //Enable plugin in app/Config/bootstrap.php
    CakePlugin::load('ProUtils');
    
## Requirements ##

* PHP version: PHP 5.3+
* CakePHP version: 2.1

## License ##

Copyright 2011-2012, [PRONIUQE Software](http://pronique.com)

Licensed under [The MIT License](http://www.opensource.org/licenses/mit-license.php)<br/>
Redistributions of files must retain the above copyright notice.

--------------------------------------------------------------------------
ProUtils is Open Source Software created and managed by PRONIQUE Software.

http://www.pronique.com
