"# �bNoteBook�W����MPI�{�����O" 
�F�ѡI�p�G�z�S���{���� MPI �{�����X�A�o�̷|�[�J�@��²�檺 MPI �d�ҵ{���A�åB�ھڦ��d�Ҩӧ勵���e�����O�C�o�����O�N�]�A�q�Y�}�l�s�g MPI �{�����B�J�C

---

### �B�J 1�G���U�öi�J Play with Docker
1. **���U�M�n��**
   - �X�� [Play with Docker](https://labs.play-with-docker.com/) �����C
   - �ϥαz�� **Google �b��** �i��n���C

2. **�Ыطs����**
   - �n�����I�� **"Start a new session"**�C
   - �Ыؤ@�ӷs���������׺ݡC

---

### �B�J 2�G�Ыبó]�m Docker ����
MPI �ݭn�e�����������p�A�����ݭn�Ыؤ@�Ӧ۩w�q�� Docker �����C

1. �b�׺ݿ�J�H�U�R�O�ӳЫغ����G

   ```bash
   docker network create mpi_network
   ```

   �p�G�o�@�B���\�A�z�|�ݨ������H�U���T���G
   ```
   mpi_network
   ```

---

### �B�J 3�G�U�� MPI Docker �M��
�b Docker ���B�� MPI �{�����e�A�ݭn�ϥξA�X�� MPI �M���C�z��ܤF `mfisherman/openmpi` �o�ӬM���C

1. �U�� MPI Docker �M���G

   ```bash
   docker pull mfisherman/openmpi
   ```

   �o�|�q Docker Hub �Ԩ� `mfisherman/openmpi` �M���C

---

### �B�J 4�G�ЫةM�B�� MPI �e��
���U�ӡA�z�ݭn�ЫبùB��X�Ӯe���A�����̤��۳s���A�ð��� MPI �{���C

1. **�ЫبùB��T�� MPI �e��**�G
   
   ��J�H�U�R�O�ӹB��T�Ӯe���A�ýT�O���̳��[�J�ۦP�������C

   ```bash
   docker run -d --name mpi_container_1 --network mpi_network mfisherman/openmpi
   docker run -d --name mpi_container_2 --network mpi_network mfisherman/openmpi
   docker run -d --name mpi_container_3 --network mpi_network mfisherman/openmpi
   ```

2. �p�G�e���Ұʦ��\�A�z���ӷ|�ݨ�e�� ID�]�p `32c0cd9ccccb...`�^����X�C

3. **�ˬd�e�����A**�G
   
   �ϥΥH�U�R�O�Ӭd�ݮe���O�_�B��G

   ```bash
   docker ps
   ```

   �o������ܥX�T�ӹB�椤���e���C�p�G�S���A�ШϥΥH�U�R�O�d�ݩҦ��e���]�]�A�w����e���^�G

   ```bash
   docker ps -a
   ```

   �Y�e�����B��A�d�ݮe����x�ӧ�X���~�G

   ```bash
   docker logs <container_name>
   ```

   �o�˯����U�z�d�ݮe���ҰʹL�{�������~�H���C

---

### �B�J 5�G�s�g²�檺 MPI �{��
�p�G�z�S�� MPI �{���A�o�̴��Ѥ@��²�檺 **Hello World** �{���d�ҡA�z�i�H�Υ��Ӵ��ձz�� MPI ���ҡC

1. **�Ы� `hello_mpi.c` �{��**�G

   �i�J����@�Ӯe���A�óЫؤ@�ӷs�� C �{�� `hello_mpi.c`�G

   ```bash
   docker exec -it mpi_container_1 bash
   nano hello_mpi.c
   ```

   �M���J�H�U²�檺 MPI �{���X�G

   ```c
   #include <stdio.h>
   #include <mpi.h>

   int main(int argc, char *argv[]) {
       int rank, size;

       // ��l�� MPI
       MPI_Init(&argc, &argv);
       
       // �����e�i�{�� rank �M�`�i�{�ƶq
       MPI_Comm_rank(MPI_COMM_WORLD, &rank);
       MPI_Comm_size(MPI_COMM_WORLD, &size);

       // �C�Ӷi�{���L�ۤv���T��
       printf("Hello from process %d of %d\n", rank, size);

       // ���� MPI
       MPI_Finalize();
       
       return 0;
   }
   ```

   �o�ӵ{���N�|��l�� MPI ���ҡA�����C�Ӷi�{���L�X�� `rank`�]�i�{�s���^�M�`�i�{�ƶq `size`�C

2. **�O�s�ðh�X**�G
   - �� `Ctrl + X` �O�s�ðh�X `nano` �s�边�C

---

### �B�J 6�G�sĶ MPI �{��
1. **�sĶ�{��**�G

   �b�e�����sĶ `hello_mpi.c` �{���G

   ```bash
   mpicc hello_mpi.c -o hello_mpi
   ```

   �o�|�ͦ��i�����ɮ� `hello_mpi`�C

---

### �B�J 7�G�B�� MPI �{��
1. **�B�� MPI �{��**�G

   �i�� MPI �B��A�ϥΥH�U�R�O�ӱҰʦh�Ӯe�������i�{�G

   ```bash
   mpirun -np 3 --host mpi_container_1,mpi_container_2,mpi_container_3 ./hello_mpi
   ```

   �o�|�ҰʤT�Ӷi�{�A���O�b `mpi_container_1`�B`mpi_container_2` �M `mpi_container_3` �W����C

---

### �B�J 8�G�ѨM�J�쪺���D
�z�ثe�J�쪺���D�O��ܪ��T�����G

```
Hello from process 0 of 1
Hello from process 0 of 1
Hello from process 0 of 1
Hello from process 0 of 1
```

�o����z���M�ҰʤF�T�Ӯe���A���Ҧ��i�{���B��b�P�@�Ӯe�����]�i�{ `0`�^�A�o�O�]�� MPI �S�����T�a�ѧO�X�e�����������G�C

**��]**�G�o�q�`�O�ѩ�H�U�X�ӭ�]�y�����G
1. **MPI �����T�Ұʦh�e��**�G�i��O�]���e���������q�H���]�m���T�C
2. **MPI �b��@�e�����B��**�G�z�i��b�C�Ӯe�������O�ҰʤF MPI �{���A�� MPI �q�{�u�B��@�Ӷi�{�]�q�`�O�i�{ `0`�^�C
3. **�����t�m���D**�G�e�������������t�m�i�ण���T�A�ɭP MPI �L�k��e���Ұʦh�Ӷi�{�C

### �ѨM��k�G
1. **�ˬd�e�������A**�G�T�O�Ҧ��e�����b�B��]�ϥ� `docker ps` �T�{�^�C
2. **�ˬd MPI �t�m**�G
   - �T�O�e�������ब�۳X�ݡC
   - �ˬd�O�_��������Φw���]�m����e���������q�H�C
3. **�ϥΥ��T���R�O�B�� MPI**�G�ϥ� `mpirun` �� `mpiexec` �ýT�O���w�e���W�١A�p�W���B�J�ҥܡC

---

### �U�@�B�G
1. �ˬd�e���B�檬�A�ýT�O�e����������q�H�C
2. �ˬd�e����x�A��X������ MPI �{���b�h�e�����������T�B��C
3. �A�����չB�� MPI �{���A�T�O�ϥΥ��T�� `mpirun` �R�O�ӱҰʦh�Ӯe�������i�{�C

---

�o�O���㪺�B�J�M�@�ǱƬd��k�A���U�z�~��i�� MPI �{�����sĶ�M����C�p��������D�A���H�ɴ��ѧ�h�H���A�ڱN���U�z�ѨM�C