


```javascript
var observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {    
        if(mutation.attributeName == 'title'){
            updateClonedNodes(mutation.target)
        }
    });    
});

var config = { attributes: true, subtree: true, attributeFilter: ['title'] };

for(var i = 0; i < arguments.length; i++){
    observer.observe(arguments[i][0], config);
}

function updateClonedNodes(targerNode){

    var $targetNode = $(targerNode);
    var $mcsItemNode = $targetNode.parents('.mcs-item');
    var productItemNode = $targetNode.parents('.product-item');
    var $mcsItemsContainer = $(targerNode).parents('.mcs-items-container'); 
    var mcsItemNumber = $mcsItemNode.data('item');
    var productIndex = $mcsItemsContainer.children('.mcs-item').index($mcsItemNode);

    $mcsItemsContainer.children('.mcs-item').each(function(ind){
        var localNumber = $(this).data('item');
        if(localNumber == mcsItemNumber && productIndex != ind){
            $(this).empty();
            var newNode = productItemNode.clone(true);
            $(this).append(newNode);
        }
    })
}
```