

Разные способы
# Init 
Для того чтобы клонировать весь элемент и вставлять в нужные ноды

# Update
Для того чтобы применить туже вью модель на клонированные елементы

```javascript
var changedProductsSubscriptions = {};

ko.bindingHandlers.observeAllMagicScrollItems = {
    init: function (element, valueAccessor, allBindingsAccessor, viewModel, bindingContext) {

        if(changedProductsSubscriptions[viewModel.id]){
            changedProductsSubscriptions[viewModel.id].dispose();
        }

        var subscriptionItemQuantity = viewModel.itemQuantity.subscribe(updatMagicScrollItems);
        var subscriptionSelectedSKU = viewModel.selectedSKU.subscribe(updatMagicScrollItems);

        changedProductsSubscriptions[viewModel.id] = subscriptionSelectedSKU;

        function updatMagicScrollItems(){
            var parentItem = $(element).parents('.mcs-item');
            var mainElNumber = parentItem.data('item');
            var itemsArray = $(element).parents('.mcs-items-container');

            var elInd = itemsArray.children().index(parentItem);

            itemsArray.children('.mcs-item').each(function(ind){
                var el = this;
                var elNumber = $(this).data('item');
                if(mainElNumber == elNumber && elInd != ind){
                    setTimeout(function(){
                        $(el).empty();
                        var newNode = element.cloneNode(true);
                        $(el)[0].appendChild(newNode);
                    }, 1000);
                }
            });
        }
    
    },
    update: function (element, valueAccessor, allBindingsAccessor, viewModel, bindingContext) {

        var itemNumber = $(element).parents('.mcs-item').data('item');
        var mcsItemsContainer = $(element).parents('.mcs-items-container'); 
        console.log(itemNumber)

        setTimeout(function(){

            var elCont = ko.contextFor(element)
            console.log(ko.contextFor(element))
            var rr = ko.observable(ko.mapping.fromJS(elCont))
            var parentItem = $(element).parents('.mcs-item');

            var itemsArray = $(element).parents('.mcs-items-container');

            var elInd = itemsArray.children().index(parentItem);

            var itemNumber = $(element).parents('.mcs-item').data('item');
            var mcsItemsContainer = $(element).parents('.mcs-items-container'); 
            mcsItemsContainer.children('.mcs-item').each(function(ind){
                var localNum = $(this).data('item');
                if(localNum == itemNumber && elInd != ind){
                    var ch = $(this).children('.product-item');
                    ch.attr('data-bind', 'with: ' + viewModel)

                }
            })
        }, 5000);

        if(subscribeArray[viewModel.id]){
            subscribeArray[viewModel.id].dispose();
        }

        var sub = viewModel.selectedSKU.subscribe(function(val){
            console.log('sdflkjsdkfjh ')
        })

        subscribeArray[viewModel.id] = sub;

    }
};

```

# Working solution
```javascript
var changedProductsSubscriptions = {};
var observeChangeArray = ['itemQuantity', 'selectedSKU'];

ko.bindingHandlers.observeAllMagicScrollItems = {
    init: function (element, valueAccessor, allBindingsAccessor, viewModel, bindingContext) {

        var productId = viewModel.id;
        
        if(!changedProductsSubscriptions[productId]) {
            changedProductsSubscriptions[productId] = {};
        }
        
        observeChangeArray.map(function(observeProperty){
            if(changedProductsSubscriptions[productId][observeProperty]){
                changedProductsSubscriptions[productId][observeProperty].dispose();
            }
            changedProductsSubscriptions[productId][observeProperty] = viewModel[observeProperty].subscribe(syncMagicScrollItems);
        });

        function syncMagicScrollItems(){
            var $mcsItem = $(element).parents('.mcs-item');
            var $mcsItemsContainer = $(element).parents('.mcs-items-container');
            var mcsItemNumber = $mcsItem.data('item');
            var mcsItemIndex = $mcsItemsContainer.children().index($mcsItem);

            $mcsItemsContainer.children('.mcs-item').each(function(mappedItemIndex){
                var $mappedItem = $(this);
                var mappedItemNumber = $mappedItem.data('item');
                if(mcsItemNumber == mappedItemNumber && mcsItemIndex != mappedItemIndex){
                    setTimeout(function(){
                        $mappedItem.empty();
                        var clonedNode = element.cloneNode(true);
                        $mappedItem.append(clonedNode);
                    }, 1000);
                }
            });
        }
    }
};
```