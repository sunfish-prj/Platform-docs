#####################################
Secure Multiparty Computation (SMC)
#####################################

This is a page for Secure Multi-party Computation (SMC) component of the SUNFISH platform. SUNFISH platform uses `Sharemind MPC <https://sharemind.cyber.ee/>`_ as a practical implementation of SMC.

We first introduce the concept of SMC, then the Sharemind approach.  

Intro to SMC
=============

Secure multi-party computation (SMC, also abbreviated as MPC) is a technique for evaluating a function with multiple peers so that each of them learns the output value but not each other's inputs. There are various ways for implementing secure MPC with different number of peers and security guarantees. Here, we concentrate on systems based on secret sharing.

Share computing systems use the concept of secret sharing introduced by Blakley (Blakley, 1979) and Shamir (Shamir, 1979). In secret sharing, a secret value :math:`s` is split into any number of shares :math:`s_1`, :math:`s_2`, :math:`\ldots`, :math:`s_n` that are distributed among the peers (computing nodes). Depending on the type of scheme used, the original value can be reconstructed only by knowing all or a predefined number (threshold :math:`t`) of these shares. Any group of :math:`t` or more peers can combine their shares to reconstruct the original value. However, the result of combining less than :math:`t` shares provides no information about the value they represent.

Secure multi-party computation protocols can be used to process secret-shared data. These protocols take secret-shared values as inputs and output a secret-shared result that can be used in further computations. For example, let us have values :math:`u` and :math:`v` that are both secret-shared and distributed among all the peers so that each computation node :math:`\mathcal{C}_i` gets the shares :math:`u_i` and :math:`v_i`. To evaluate :math:`w = u \oplus v` for some binary function :math:`\oplus`, the computation nodes engage in a share computing protocol and output :math:`w` in a shared form (node :math:`\mathcal{C}_i` holds :math:`w_i`). During the computation, no computation node is able to recover the original values :math:`u` or :math:`v` nor learn anything about the output value :math:`w`.

.. figure:: images/smc-computation.png
   :width: 70%
   :align: center

   An example of secret sharing two values (orange and blue) among three computation nodes and an SMC protocol that results in a secret-shared result (green).

The fact that most SMC protocols are composable and the computation result is also secret-shared, allows one to use the output of one computation as an input for the next one. Using this property, one can combine primitive functions like multiplication or comparison into algorithms (e.g. sorting) and such algorithms into applications that implement the necessary business logic in privacy-preserving fashion.

Multi-party computation protocols can be secure in either passive or active corruption models. In the passive model, an adversary can read all the information available to the corrupted peer, but it cannot modify it. In this case, the corrupted peer still follows the predefined protocol, but it tries to deduce the original data values based on the information available to that peer. This is also known as *honest-but-curious* model.
In the active model, an adversary has full control over the corrupted peer. For more properties of SMC protocols, see Cramer *et al.*, 2004.


Sharemind
===========

Sharemind MPC is a practical implementation of secure multi-party computation technology with the emphasis on performance and ease of use. Sharemind MPC supports several different SMC schemes called *protection domains*, but the SUNFISH platform uses the *shared3p* protection domain, which stands for 3-out-of-3 secret sharing with passive security. This protection domain uses additive secret sharing scheme, where a secret value :math:`s` is secret shared as follows:

.. math::
   s_1 &\leftarrow \mathrm{random()},\\
   s_2 &\leftarrow \mathrm{random()},\\
   s_3 &\leftarrow s - s_1 - s_2,

such that :math:`s = s_1 + s_2 + s_3`. All these computations are done modulo the corresponding data type size, e.g. modulo :math:`2^{64}` for 64-bit (unsigned) integers. Note that this modulo computation happens automatically for primitive data types like ``(u)int8``, ``(u)int16``, ``(u)int32`` and ``(u)int64``. More complex data types (e.g. floating point numbers) use structures of primitive data types.

Architecture
------------

Sharemind MPC deployment consists of two types of components -- application servers and client applications. 
Sharemind Application Server implements the SMC computation node role, it talks to other Application Servers during SMC protocols and to client applications for user input and output. It also has a local persistent storage for saving input shares and computation results between computations.
A typical Sharemind MPC deployment supporting the *shared3p* protection domain has three application servers and any number of client applications.

On Sharemind MPC platform, privacy-preserving applications are developed using the open source SecreC programming language. SecreC is a domain specific language that separates private and public data flows. By marking user (and other sensitive) input as private, an application developer can be confident that all computations involving these values are executed in the secure SMC environment. At the same time, the developer does not have to know the underlying SMC protocol details.

An example SecreC program, counting the number of occurrences of a secret value in a vector of secret values:

.. code-block:: c++

   import shared3p;
   import stdlib;

   // All variables prefixed with `pd_shared3p` are secret-shared
   domain pd_shared3p shared3p;

   void main() {
      pd_shared3p uint64[[1]] haystack = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
      // In reality, this comes from database:
      // pd_shared3p uint64[[1]] haystack = loadFromDB(...);
      pd_shared3p uint64 needle = 5;
      // In reality, this comes from user-supplied argument, for example:
      // pd_shared3p uint64 needle = argument("needle");

      // Because of private operands, the equality operation
      // invokes a corresponding SMC protocol
      pd_shared3p bool[[1]] match = (needle == haystack);

      // Publish orders each computation node to send its 
      // corresponding share back to the client application
      publish("count", sum(match));
   }

SecreC programs are deployed on Application Servers and invoked by authorised client applications by their name (think of remote procedure calls or stored procedures in database management systems). This happens in parallel in all three Application Servers.

.. figure:: images/smc-data-usage-policy.png
   :width: 60%
   :align: center

   In Sharemind MPC, each Application Server is independent in validating the user query against its access control list (ACL) and the data usage policy.

It is important to notice that each Application Server is independent in deciding
a) whether the user is authorised to run a given SecreC program; and
b) if the requested SecreC program correctly implements the data usage policy.
An SMC protocol cannot be executed if the Application Servers do not reach consensus in these questions. Consequently, a user can only run a predetermined set of programs and a single server or a pair of servers cannot allow potentially malicious queries without the consent of the third server.
This provides cryptographic enforcement of data usage policies.

Requirements and Privacy Guarantees
-----------------------------------

Deploying Sharemind MPC in practice requires that the three Application Servers (computation nodes) are hosted by independent parties who do not collude. Good candidates are government organisations from different jurisdictions or peers that are themselves interested in the correct outcome of the computation.

With the non-collusion requirement holding, secure multi-party computation technology and Sharemind MPC guarantee the confidentiality of private values, except the ones that are explicitly published by all three servers (either to the user or the servers themselves).


| **Contact information:**
| Riivo Talviste
| riivo.talviste@cyber.ee
|
