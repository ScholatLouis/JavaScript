###substring和substr的区别
1 substring(from, to) : 截取字符串从from到to-1的位置，小标从0开始，对于原始的字符串并不会改变而是返回一个新的字符串。   
2 substr(from, length): 同样是截取字符串，从from位置开始，第二个参数不再是位置参数，而是规定了截取的长度为length，同样不会修改原始字符串,
而是返回一个新的字符串。
