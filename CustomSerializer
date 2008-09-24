= XML-RPC Custom Serializer =
Redstone XML-RPC has the ability to add custom serializer classes to convert our model objects to XML-RPC structs.
Here are the steps to adding a new custom serializer. We used to convert objects into !HashMaps then allow Redstone to serialize the maps, but this required a two stage conversion which is costly.

 * implement [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html XmlRpcCustomSerializer]
 * [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html#getSupportedClass() getSupportedClass] should return the class you can serialize.
 * the serialize method should make use of other serializers by calling [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html#serialize(java.lang.Object,%20java.io.Writer,%20redstone.xmlrpc.XmlRpcSerializer) XmlRpcSerializer.serialize]

== Quick steps ==
 * implement [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html XmlRpcCustomSerializer]
 * return the class you support from [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html#getSupportedClass() getSupportedClass]
 * implement serialize method to append XML-RPC formatted text to the !StringBuffer
 * add an entry to {{{SerializerRegistry.java}}}
 * DONE

== Example ==
{{{
#!java
/**
 * ManagedServerGroupSerializer is a custom serializer for the XMLRPC library.
 * It converts an ServerGroup to an XMLRPC &lt;struct&gt;.
 * @version $Rev$
 * @xmlrpc.doc
 *      #struct("Server Group")
 *          #prop("int", "id")
 *          #prop("string", "name")
 *          #prop("string", "description")
 *          #prop("int", "org_id")
 *          #prop("int", "system_count")
 *      #struct_end()
 *          
 */
}}}

Every serializer needs to document what it will output in terms of [http://www.xmlrpc.com/spec#scalars XML-RPC data structures]. Look around at other serializers to see the different documentation types. This javadoc is very important as this is how we auto-generate the API docs.

{{{
#!java
public class ManagedServerGroupSerializer implements XmlRpcCustomSerializer {

    public static final String MAX_MEMBERS = "max_system_count";
    public static final String CURRENT_MEMBERS = "system_count";

    /** {@inheritDoc} */
    public Class getSupportedClass() {
        return ManagedServerGroup.class;
    }
}}}

The [http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html#getSupportedClass() getSupportedClass] should return the top most class that this serializer can serialize. For instance,
to serialize a {{{List}}}, {{{LinkedList}}}, etc. you'd return {{{Collection.class}}}.

{{{
#!java
    /**
     * Converts a ServerGroup to a XMLRPC &lt;struct&gt; containing the top-
     * level fields of the ServerGroup. It serializes the Org as just an ID
     * instead of traversing the entire object graph.
     * @param value ServerGroup object.
     * @param output Buffer to serialize the object to.
     * @param builtInSerializer basic XMLRPC serializer
     * @throws XmlRpcException thrown if a problem occurs with serializing
     * the value.
     * @throws IOException thrown if a problem occurs with serializing
     * the value.
     */
    public void serialize(Object value, Writer output, XmlRpcSerializer builtInSerializer)
        throws XmlRpcException, IOException {

        ServerGroup sg = (ServerGroup) value;

        SerializerHelper serializer = new SerializerHelper(builtInSerializer);
        serializer.add("id", sg.getId());
        serializer.add("name", sg.getName());
        serializer.add("description", sg.getDescription());
        serializer.add(CURRENT_MEMBERS, sg.getCurrentMembers());
        serializer.add("org_id", sg.getOrg().getId());
        serializer.writeTo(output);
    }
}}}

[http://xmlrpc.sourceforge.net/javadoc/redstone/xmlrpc/XmlRpcCustomSerializer.html#serialize(java.lang.Object,%20java.io.Writer,%20redstone.xmlrpc.XmlRpcSerializer) XmlRpcSerializer.serialize] is used to serialize common java.lang objects. The main thing to consider is how 
far down the object graph to go.
