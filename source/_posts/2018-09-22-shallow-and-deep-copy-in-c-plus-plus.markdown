---
layout: post
title: "Shallow and Deep Copy in C++ Language"
date: 2018-09-22 00:42:20 -0300
comments: true
categories: c/c++
---

When we create a new object in C++, the default constructor already do the process known as **memberwise copy** or
commonly **shallow copy**. It happens, because C++ compiler<!--more--> do not known enough about the current class that
is being evaluated in run time.

For instance, consider the hypothetical example below:

{% highlight c++ %}
namespace Network
{
        class IPConfig
        {
                private:
                        const char *m_ip_address;
                        const char *m_mask;
                public:
                        IPConfig( const char * ip_address, const char * mask );

                        char * get_ip_address() const;
                        const char * get_mask() const;
        }
}
{% endhighlight %}

And here is implementation of the one's own default constructor mentioned previously above:

{% highlight c++ %}
Network::IPConfig::IPConfig( const char *ip_address, const char *mask )
                : m_ip_address( ip_address ), m_mask( mask ) {}
{% endhighlight %}

How we can see above, we have an example code snippet that show a conventional code of a class that represent an
Internet Protocol(IP), used in network communication layer. This class have a default constructor that configure
the members variables with values given in your parameters.

When we create a new object, like this:

{% highlight c++ %}
Network::IPConfig ipconfig( "192.168.0.1", "255.255.0.0" );
{% endhighlight %}

and then we do like this:

{% highlight c++ %}
read_ipconfig( ipconfig );
{% endhighlight %}

The compiler behind the curtains, will do something like it when we handle it on **read_ipconfig** function:

{% highlight c++ %}
Network::IPConfig::IPConfig( const Network::IPConfig & ipconfig )
{
	 m_address = ipconfig.m_ip_address;
	 m_mask = ipconfig.m_mask;
}
{% endhighlight %}

Look that the compiler created a new object, based from state another object of same type. Notice also that, the constructor
default is not initialized again, therefore, object already has a initialized state. As we have not our own copy
constructor, then the compiler use own way to performs member-wise copy, according shown above. Okay, so far we have not
problem here, it is a default feature of own C++ compiler.

However, the problem begins emerge when we work with allocation of memory blocks and we have a destructor in our class that
deallocates eventual blocks of memory previously allocated, when our object is no more referenced on present scope.

Let's go change the constructor previously shown above and implements it with allocate blocks, after we will deallocate the memory
of block allocated:

{% highlight c++ %}
Network::IPConfig::IPConfig( const char *ip_address, const char *mask )
                : m_ip_address( ip_address ), m_mask( mask )
{
        assert( m_ip_address );
        assert( m_mask );

        int s_size = std::strlen( m_ip_address );
        m_buffer_ip_address = ( char * ) malloc( s_size + 1 );
        for ( int i = 0; i < s_size; ++i )
                m_buffer_ip_address[ i ] = m_ip_address[ i ];
}
{% endhighlight %}

{% highlight c++ %}
Network::IPConfig::~IPConfig()
{
        free( m_buffer_ip_address );
}
{% endhighlight %}

Now, let us suppose that we have a non-member function that validate if the ip address given by parameter it's an
Internet Protocol version 4 valid format:

{% highlight c++ %}
bool
is_valid_ipv4( const Network::IPConfig ipconfig )
{
        std::stringstream ip( ipconfig.get_ip_address() );
        std::vector<std::string> split_ip;

        std::string field;
        while( std::getline( ip, field, '.' ) )
                split_ip.push_back( field );

        if ( split_ip.size() == 4 )
                return true;
        return false;
}
{% endhighlight %}

Without going into detail, for those that no known, the internet Protocol version 4 (IPv4), it's have the ip address format with
quad-dotted representations( 32 bits ). Roughly, it's separated by four dots. ***eg: 200.201.3.33***

After we validate and ensure that given ip address is an version 4 accept, we will create an another non-member function
that simply reads and show the ip address, together with your mask set up:

{% highlight c++ %}
void
read_ipv4( const Network::IPConfig ipc )
{
        std::cout << ipc.get_ip_address() << "/" << ipc.get_mask() << std::endl;
}
{% endhighlight %}

Finally, let go create the main function that will go call the non-member function that will validate the ip address
and after, will read the value with mask concatenation:

{% highlight c++ %}
int
main( void )
{
        Network::IPConfig ipconfig( "192.168.1.30", "255.255.255.0" );
        if ( is_valid_ipv4( ipconfig ) )
                read_ipv4( ipconfig );
        else
                std::cout << "Invalid ipv4 format." << std::endl;
}
{% endhighlight %}

Therefore, the expect output of the read_ipv4 function is the IP Address and your mask, like this:

**192.168.1.30/255.255.255.0**

but it did not work as expected, we had an unexpected behavior and nothing was read. Do you know why this happened?

```c++
#include <cstring>
#include <cassert>
#include <iostream>
#include <sstream>
#include <vector>

#include <stdlib.h>

// The header
namespace Network
{
        class IPConfig
        {
                private:
                        const char *m_ip_address;
                        char *m_buffer_ip_address;
                public:
                        IPConfig( const char * );
                        ~IPConfig();

                        char * get_ip_address() const;
        };
}

/**
 * Constructor default. This will allocate memory blocks to
 * IP Address target.
 */
Network::IPConfig::IPConfig( const char *ip_address )
                : m_ip_address( ip_address )
{
        assert( m_ip_address );
        int s_size = std::strlen( m_ip_address );
        m_buffer_ip_address = ( char * ) malloc( s_size + 1 );
        for ( int i = 0; i < s_size; ++i )
                m_buffer_ip_address[ i ] = m_ip_address[ i ];
}

/**
 * Destructor. This will destroy allocation.
 */
Network::IPConfig::~IPConfig()
{
        free( m_buffer_ip_address );
}

/**
 * Gets the configured IP Address.
 */
char *
Network::IPConfig::get_ip_address() const
{
        return m_buffer_ip_address;
}


// The main part. Here we will go to test the Network IPConfig feature.

bool
is_valid_ipv4( const Network::IPConfig ipc )
{
        std::stringstream ip( ipc.get_ip_address() );
        std::vector<std::string> split_ip;

        std::string field;
        while( std::getline( ip, field, '.' ) )
                split_ip.push_back( field );

        if ( split_ip.size() == 4 )
                return true;
        return false;
}

void
read_ipv4( const Network::IPConfig ipc )
{
        std::cout << ipc.get_ip_address() << std::endl;
}

int
main( void )
{
        Network::IPConfig ipc( "192.168.1.30" );
        if ( is_valid_ipv4( ipc ) )
                read_ipv4( ipc );
        else
                std::cout << "Invalid ipv4 format." << std::endl;
}
```

In the main function, we create a Network::IPConfig type, configured with ip address 192.168.1.30 and, soon after,
we validate if it's a valid IP Address format version 4. The IP address version 4, is separeted by 4 dots, if this
match, the is_valid_ipv4 function will return true, otherwise false.

So, after, we will read the IP Address sets by constructor. Therefore, the expect output of the read_ipv4 function
is the IP Address 192.168.1.30, but, we have a unexpected behavior. Why did this happen?

It happens, because the compiler 
