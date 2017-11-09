# Huffman
function Node(name, fr, used, code, parent_id) {
	this.name = name;
	this.fr = fr;
	this.used = used;
	this.code = code;
	this.parent_id = parent_id;
}

str = WScript.StdIn.ReadLine(); 
alph= new Array();
code_tab = new Array();
tree = new Array();
code_mes = "";
str2 = "";
decoded_mes = ""; 

for(i=0;i<str.length;i++)
	alph[str.charAt(i)]=0;

for(i=0;i<str.length;i++)
	alph[str.charAt(i)]++; 

for(i in alph){
	n = new Node(i, alph[i], 0, '', null);
	tree.push(n);
}  

m = tree.length;

if (m < 2) {
	tree[0].code = 0;
	code_tab[tree[0].name] = 0;
	}
	
function Min_Frequency() {
	fr = str.length;
	node = 0;
	for (i=0; i<tree.length; i++)
		if ((tree[i].fr < fr) && (tree[i].used == 0 ))
		{
			fr = tree[i].fr;
			node = i;
		}
	tree[node].used = 1;
	tree[node].parent_id = tree.length;
	return node;
}
	
for(k=1;k<m;k++){
	node_left = Min_Frequency();
	tree[node_left].code = 0;	
	
	node_right = Min_Frequency();
	tree[node_right].code = 1;
	
	n = new Node(tree[node_left].name + tree[node_right].name, tree[node_left].fr + tree[node_right].fr, 0, '', null);
	tree.push(n);
}

if (m > 1) {
for(i=0;i<m;i++){
	j = i;
	code_tab[tree[j].name] = '';

	while(tree[j].parent_id){
		code_tab[tree[i].name] = tree[j].code + code_tab[tree[i].name];
		j = tree[j].parent_id;
	}
}
}

for(i = 0; i< str.length; i++){
	for (j in code_tab)
	{
		if (str.charAt(i) == j)
			code_mes += code_tab[j];
	}
}

WSH.echo(code_mes);

for (i = 0; i < code_mes.length; i++)
{
	str2 += code_mes.charAt(i);
	for (j in code_tab)
	{
		if (str2 == code_tab[j])
		{
			decoded_mes+=j;
			str2 = "";
		}
	}
}
WSH.echo(decoded_mes);
WSH.echo();

WSH.echo('name \t code');
for(i in code_tab)
	WSH.echo(i,'\t',code_tab[i]);

WSH.echo();
WSH.echo('name \t freq \t code \t parent_id');
for(i=0;i<tree.length;i++)
	WSH.echo(tree[i].name, '\t',tree[i].fr, '\t', tree[i].code, '\t', tree[i].parent_id);
WSH.echo();
