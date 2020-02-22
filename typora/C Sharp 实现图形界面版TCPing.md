# C Sharp 实现图形界面版TCPing

界面设计

![](C#%20%E5%AE%9E%E7%8E%B0%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2%E7%89%88TCPing.assets/image-20200222180320422-1582366668142.png)

代码

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.Threading;
using System.Net.Sockets;

namespace TCPing
{
    public partial class Form1 : Form
    {

        private string okText = "连通了";
        private Color okColor = Color.Green;
        private string failText = "连不通";
        private Color failColor = Color.Red;
        private bool status = false;
        private string excMsg = "";

        public Form1()
        {
            InitializeComponent();
        }

        private void ping_Click(object sender, EventArgs e)
        {
            result.ForeColor = Color.Black;
            result.Text = "正在测试...";
            using (BackgroundWorker bw = new BackgroundWorker())
            {
                bw.RunWorkerCompleted += new RunWorkerCompletedEventHandler(changeResilt);
                bw.DoWork += new DoWorkEventHandler(onClickPing);
                bw.RunWorkerAsync();
            }
            ping.Enabled = false;
        }

        private void changeResilt(object sender, RunWorkerCompletedEventArgs e)
        {
            if (status)
            {
                result.ForeColor = okColor;
                result.Text = okText;
            }
            else
            {
                result.ForeColor = failColor;
                result.Text = failText + ":" + excMsg;
            }
            ping.Enabled = true;
        }

        private void onClickPing(object sender, DoWorkEventArgs ev)
        {
            string host = this.hostname.Text;
            int port = Int32.Parse(this.port.Text);
            status = connect(host, port);
        }

        private bool connect(string host, int port)
        {
            bool ret = false;
            try
            {
                TcpClient tcp = new TcpClient(host, port);               
                NetworkStream s = tcp.GetStream();
                bool b = s.CanRead;
                ret = true;
                s.Close();
                tcp.Close();                
            }
            catch (Exception e)
            {
                excMsg = e.Message;
            }

            return ret;
        }
    }
}

```

运行效果测试

![](C#%20%E5%AE%9E%E7%8E%B0%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2%E7%89%88TCPing.assets/image-20200222180411386.png)

![](C#%20%E5%AE%9E%E7%8E%B0%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2%E7%89%88TCPing.assets/image-20200222180335217.png)

<a href="TCPing.exe" >下载</a>

