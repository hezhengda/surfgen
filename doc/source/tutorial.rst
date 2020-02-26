Tutorial
========

Example 0: Import surfgen package
---------------------------------

The main functionalities in surfgen package are all located in **surfgen.core** module.

.. code-block:: Python

   from surfgen.core import slab_generator, surf_atom_finder, connection_matrix, get_coordination_number, find_all_ads_sites, align_adsorbate_one_atom, align_adsorbate_multiple_atoms

Example 1: Create the (3x3x1) supercell for Cu(111) surface and find all adsorption sites
-----------------------------------------------------------------------------------------

.. code-block:: Python

    # ase
    from ase.build import molecule, bulk
    from ase.visualize import view
    from ase.io import read
    from ase.constraints import FixAtoms

    # python module
    import copy
    import numpy as np

    lc_Cu = 3.579 # lattice parameter of Cu bulk
    d_Cu = lc_Cu / np.sqrt(2) # closest distance between two Cu atoms

    Cu_bulk = bulk('Cu', 'fcc', lc_Cu)

    # slab_generator will return a list of slab surfaces, we should check all of them first and then determine which one we want
    slab_list = slab_generator(bulk=Cu_bulk, miller_index=(1, 1, 1), slab_height=8.0, vacuum=18.0, supercell=(3, 3, 1))
    slab = slab_list[0]
    view(slab)

    # use surf_atom_finder to find all atoms in first surface layer
    surf_atoms = surf_atom_finder(slab)
    slab_cpy = copy.deepcopy(slab)
    c = FixAtoms(indices = [atom.index for atom in slab_cpy if atom.index in surf_atoms])
    slab_cpy.set_constraint(c)
    view(slab_cpy)
    print(surf_atoms)

    # create connection_matrix
    connector, conn_coordinates, outer_atom_index = connection_matrix(slab, surf_atoms, d_Cu)

    # find coordination number
    coord_num = get_coordination_number(connector)

    # find all adsorption site
    ads_sites = find_all_ads_sites(slab, connector, conn_coordinates, surf_atoms, d_Cu)
    print(ads_sites)

For the example, you will get:

.. code-block:: bash

   {'ontop': {'location': [array([ 1.67828676,  0.97039162, 18.16279368]), array([ 2.93945124,  3.16126249, 18.28182945]), array([ 4.20061572,  5.35213335, 18.40086522]), array([ 4.20622089,  0.97039162, 18.28182945]), array([ 5.46738537,  3.16126249, 18.40086522]), array([ 6.72854985,  5.35213335, 18.51990099]), array([ 6.73415503,  0.97039162, 18.40086522]), array([ 7.9953195 ,  3.16126249, 18.51990099]), array([ 9.25648398,  5.35213335, 18.63893676])], 'norm_vector': [array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398])], 'label': ['3', '7', '11', '15', '19', '23', '27', '31', '35']}, 'bridge': {'location': [array([ 2.32297459,  2.07398296, 17.92275437]), array([ 2.95635942,  0.97854753, 17.92275437]), array([ 2.95635942,  0.97854753, 17.92275437]), array([ 3.58413907,  4.26485383, 18.04179014]), array([ 4.2175239 ,  3.1694184 , 18.04179014]), array([ 4.2175239 ,  3.1694184 , 18.04179014]), array([ 4.2175239 ,  3.1694184 , 18.04179014]), array([ 5.47868838,  5.36028926, 18.16082591]), array([ 5.47868838,  5.36028926, 18.16082591]), array([ 5.48429356,  0.97854753, 18.04179014]), array([ 5.48429356,  0.97854753, 18.04179014]), array([ 6.74545804,  3.1694184 , 18.16082591]), array([ 6.74545804,  3.1694184 , 18.16082591]), array([ 6.74545804,  3.1694184 , 18.16082591]), array([ 8.00662251,  5.36028926, 18.27986168]), array([ 8.00662251,  5.36028926, 18.27986168])], 'norm_vector': [array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398])], 'label': ['3.7', '3.15', '7.15', '7.11', '7.19', '11.19', '15.19', '11.23', '19.23', '15.27', '19.27', '19.31', '23.31', '27.31', '23.35', '31.35']}, 'hollow': {'location': [array([ 2.96482896,  1.71427509, 17.74288887]), array([ 4.22599344,  3.90514596, 17.86192464]), array([ 4.22786183,  2.44456538, 17.82224605]), array([ 5.48902631,  4.63543625, 17.94128182]), array([ 5.49276309,  1.71427509, 17.86192464]), array([ 6.75392757,  3.90514596, 17.98096041]), array([ 6.75579596,  2.44456538, 17.94128182]), array([ 8.01696044,  4.63543625, 18.06031759])], 'norm_vector': [array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398]), array([-0.04701866, -0.02718636,  0.99852398])], 'label': ['3.7.15', '7.11.19', '7.15.19', '11.19.23', '15.19.27', '19.23.31', '19.27.31', '23.31.35']}, 'four_fold': {'location': [], 'norm_vector': [], 'label': []}}

There are three arrays in ads_sites:

* 'location': which contains all the locations of adsorption sites

* 'norm_vector': which contains all norm_vectors of adsorption sites

* 'label': which contains the structural information about the adsorption site. It tells us which atoms form the active site
