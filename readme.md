1.在进行UDP编程的时候,我们最容易想到的问题就是,一次发送多少bytes好?
首先,我们知道,TCP/IP通常被认为是一个四层协议系统,包括链路层,网络层,运输层,应用层.     
   UDP属于运输层,下面我们由下至上一步一步来看:     
   以太网(Ethernet)数据帧的长度必须在46-1500字节之间,这是由以太网的物理特性决定的.     
   这个1500字节被称为链路层的MTU(最大传输单元).     
   但这并不是指链路层的长度被限制在1500字节,其实这这个MTU指的是链路层的数据区.     
   并不包括链路层的首部和尾部的18个字节.     
   所以,事实上,这个1500字节就是网络层IP数据报的长度限制.     
   因为IP数据报的首部为20字节,所以IP数据报的数据区长度最大为1480字节.     
   而这个1480字节就是用来放TCP传来的TCP报文段或UDP传来的UDP数据报的.     
   又因为UDP数据报的首部8字节,所以UDP数据报的数据区最大长度为1472字节.     
   这个1472字节就是我们可以使用的字节数。

当我们发送的UDP数据大于1472的时候会怎样呢？     
   这也就是说IP数据报大于1500字节,大于MTU.这个时候发送方IP层就需要分片(fragmentation).     
   把数据报分成若干片,使每一片都小于MTU.而接收方IP层则需要进行数据报的重组.     
   这样就会多做许多事情,而更严重的是,由于UDP的特性,当某一片数据传送中丢失时,接收方便     
   无法重组数据报.将导致丢弃整个UDP数据报。     
    
   因此,在普通的局域网环境下，我建议将UDP的数据控制在1472字节以下为好.  

进行Internet编程时则不同,因为Internet上的路由器可能会将MTU设为不同的值.     
   如果我们假定MTU为1500来发送数据的,而途经的某个网络的MTU值小于1500字节,那么系统将会使用一系列的机     
   制来调整MTU值,使数据报能够顺利到达目的地,这样就会做许多不必要的操作.     
    
   鉴于Internet上的标准MTU值为576字节,所以我建议在进行Internet的UDP编程时.     
   最好将UDP的数据长度控件在548字节(576-8-20)以内.     