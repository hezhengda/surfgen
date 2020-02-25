surfgen package
===============

Although in the package we have many functions, they belong to different categories:

* Necessary functions for slab generation:

    * slab_generator(bulk, miller_index, slab_height, vacuum, supercell)

         This function can generate slab from the ASE bulk object

    * surf_atom_finder(slab, upper_lower = 'upper')

         This function is used to get the surface atom

    * connection_matrix(slab, surface_atoms, bond_length):

         This function can help us determine the connection matrix in the slab, and it will return two things:

         1. The connection matrix (which contains the index information)

         2. All the coordinates (which contains all the information of coordinates)

    * find_all_ads_sites(slab, connector, conn_coordinates, surf_atoms, bond_length)

         This function can help us generate all possible adsorption sites on a surface.

    * align_adsorbate_one_atom(slab, first_layer, site, norm_vector, label, molecule, mol_index, hybridization)

         This function can align the molecule on particular site through norm_vector.

    * align_adsorbate_multiple_atoms(slab, first_layer, molecule, match_dict)

         This function can align adsorbate with multiple adsorption site on the surface with correct geometry.

Submodules
----------

surfgen.surfgen module
----------------------

.. automodule:: surfgen.surfgen
   :members:
   :undoc-members:
   :show-inheritance:


Module contents
---------------

.. automodule:: surfgen
   :members:
   :undoc-members:
   :show-inheritance:
