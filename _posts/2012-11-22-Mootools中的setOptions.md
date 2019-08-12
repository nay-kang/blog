在做一个Tree的Web控件的时候，用的mootools,建了两个对象，分别是Tree和Node，当向Tree中加入Node节点的时候，由于Node要调用到Tree的实例，所以我通过setOptions把Tree的实例传给Node

```javascript
Tree=new Class({
    addNode:function(node){
        node.setOptions({tree:this});
    }
});
tree=new Tree();
node=new Node();
tree.addNode(node);
```

其中node会在这个实例里面调用一些操作，这些操作又关联一些tree的数据。  
结果发现，虽然只有一个tree的实例，但是在node调用tree的时候，调用的却不是同一个实例，貌似setOptions方法是把options clone之后交给对象的。
在此Mark一下。有时间再好好研究一下。