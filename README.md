http://www.cnblogs.com/javawebsoa/archive/2013/04/19/3030590.html
http://bbs.csdn.net/topics/360050777

var store = Ext.create("Ext.data.TreeStore", {
	  //model : 'node.model.Node',//用model传递不了数据
	  proxy : {
	    type : 'ajax',
	    url : 'page/page1.action',
	    reader : 'json',
	    autoLoad : true
	  },
	  fields : [{
	    name: 'id',
	    type: 'string'
	  }, {
	    name: 'name',
	    type: 'string'
	  }, {
	    name: 'code',
	    type: 'string'
	  }, {
	    name: 'ip',
	    type: 'string'
	  }, {
	    name: 'port',
	    type: 'string'
	  }, {
	    name: 'desc',
	    type: 'string'
	  }]

	});
store.load({
	callback : function(records, operation, success) {
	  console.log(records);
	}
	});
var tree = Ext.create('Ext.tree.Panel', {
	  title : "manager",
	  columnLines : true,
	  region: 'center',
	  root:{
	    id:'root',
	    name:'manager',
	    expanded:true
	  },
	  columns: [{
	    xtype : 'treecolumn',
	    header: 'name',  dataIndex: 'name', sortable:false,flex:1
	  }, {
	    header: 'code', dataIndex: 'code',align : 'center',sortable:false,width:150
	  }, {
	    header: 'IP',  dataIndex: 'ip', align : 'center',sortable:false,width:150
	  }, {
	    header: 'interface', dataIndex: 'port',align : 'center',sortable:false,width:50
	  }, {
	    header: 'des', dataIndex: 'desc',align : 'center',sortable:false,width:200
	  }],
	  loadMask:{
	    msg : 'loading...'
	  },
	  store : store
	  //store : "NodeStore",
	//  renderTo: Ext.getBody()
	});
             var store1 = Ext.create('Ext.data.TreeStore', {
                 proxy: {
                     type: 'ajax',
                     url: 'page/page1.action'
                 },
                 sorters: [{
                     property: 'leaf',
                     direction: 'ASC'
                 }, {
                     property: 'text',
                     direction: 'ASC'
                 }]
             });
             var tree1 = Ext.create('Ext.tree.Panel', {
                 store: store1,
                 rootVisible: false,
                 useArrows: true,
                 frame: true,
                 title: 'Check Tree',
                 width: 289,
                 height: 220,
                 dockedItems: [{
                     xtype: 'toolbar',
                     items: {
                         text: 'Get checked nodes',
                         handler: function(){
                             var records = tree.getView().getChecked(),
                                 names = [];
                                 //为存储ID而创建数组
                                 ids   = [];
                             
                             Ext.Array.each(records, function(rec){
                                 names.push(rec.get('text'));
                                 //将选中的节点的ID保存
                                 ids.push(rec.get('id'));
                             });
                             
                             Ext.MessageBox.show({
                                 title: 'Selected Nodes',
                                 msg: names.join('<br />')+ids.join('<br />'),
                                 icon: Ext.MessageBox.INFO
                             });
                         }
                     }
                 }]
             });
Ext.application({
    name : "node",
    launch : function() {
      Ext.create("Ext.container.Viewport", {
        items : [tree, tree1]
      });
    }
  });
