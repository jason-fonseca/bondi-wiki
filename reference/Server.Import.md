takes the object referenced at the end of the included file and returns it, this makes for an interesting module system:

in module using file:

var ModuleName = Server.Import("modulename.esp");

ModuleName.test();

in module file:

new (function() {

  this.test = function() { return true; };

})();