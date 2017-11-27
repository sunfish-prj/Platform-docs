##############################
Secure Multi-party Computation
##############################

Introduction
------------

Secure multi-party computation (SMC, also abbreviated as MPC) is a technique for evaluating a function with multiple peers so that each of them learns the output value but not each other's inputs. There are various ways for implementing secure MPC with different number of peers and security guarantees. Here, we concentrate on systems based on secret sharing.

Share computing systems use the concept of secret sharing introduced by Blakley (Blakley, 1979) and Shamir (Shamir, 1979). In secret sharing, a secret value :math:`s` is split into any number of shares :math:`s_1`, :math:`s_2`, :math:`\ldots`, :math:`s_n` that are distributed among the peers (computing nodes). Depending on the type of scheme used, the original value can be reconstructed only by knowing all or a predefined number (threshold :math:`t`) of these shares. Any group of :math:`t` or more peers can combine their shares to reconstruct the original value. However, the result of combining less than :math:`t` shares provides no information about the value they represent.

Secure multi-party computation protocols can be used to process secret-shared data. These protocols take secret-shared values as inputs and output a secret-shared result that can be used in further computations. For example, let us have values :math:`u` and :math:`v` that are both secret-shared and distributed among all the peers so that each computation node :math:`\mathcal{C}_i` gets the shares :math:`u_i` and :math:`v_i`. To evaluate :math:`w = u \oplus v` for some binary function :math:`\oplus`, the computation nodes engage in a share computing protocol and output :math:`w` in a shared form (node :math:`\mathcal{C}_i` holds :math:`w_i`). During the computation, no computation node is able to recover the original values :math:`u` or :math:`v` nor learn anything about the output value :math:`w`.

.. figure:: ../images/smc-computation.png
   :width: 70%
   :align: center

   An example of secret sharing two values (orange and blue) among three computation nodes and an SMC protocol that results in a secret-shared result (green).

The fact that most SMC protocols are composable and the computation result is also secret-shared, allows one to use the output of one computation as an input for the next one. Using this property, one can combine primitive functions like multiplication or comparison into algorithms (e.g. sorting) and such algorithms into applications that implement the necessary business logic in privacy-preserving fashion.

Multi-party computation protocols can be secure in either passive or active corruption models. In the passive model, an adversary can read all the information available to the corrupted peer, but it cannot modify it. In this case, the corrupted peer still follows the predefined protocol, but it tries to deduce the original data values based on the information available to that peer. This is also known as *honest-but-curious* model.
In the active model, an adversary has full control over the corrupted peer. For more properties of SMC protocols, see Cramer *et al.*, 2004.
