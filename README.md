using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace CRUD_App
{
    public partial class Form1 : Form
    {
        SqlConnection con = new SqlConnection("Data Source=.;Initial Catalog=StudentDB;Integrated Security=True");

        public Form1()
        {
            InitializeComponent();
        }

        // LOAD DATA
        void LoadData()
        {
            SqlCommand cmd = new SqlCommand("sp_GetStudents", con);
            cmd.CommandType = CommandType.StoredProcedure;

            SqlDataAdapter da = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            da.Fill(dt);

            dataGridView1.DataSource = dt;
        }

        // INSERT
        private void btnInsert_Click(object sender, EventArgs e)
        {
            SqlCommand cmd = new SqlCommand("sp_InsertStudent", con);
            cmd.CommandType = CommandType.StoredProcedure;

            cmd.Parameters.AddWithValue("@Name", txtName.Text);
            cmd.Parameters.AddWithValue("@City", txtCity.Text);

            con.Open();
            cmd.ExecuteNonQuery();
            con.Close();

            LoadData();
        }

        // UPDATE
        private void btnUpdate_Click(object sender, EventArgs e)
        {
            int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);

            SqlCommand cmd = new SqlCommand("sp_UpdateStudent", con);
            cmd.CommandType = CommandType.StoredProcedure;

            cmd.Parameters.AddWithValue("@Id", id);
            cmd.Parameters.AddWithValue("@Name", txtName.Text);
            cmd.Parameters.AddWithValue("@City", txtCity.Text);

            con.Open();
            cmd.ExecuteNonQuery();
            con.Close();

            LoadData();
        }

        // DELETE
        private void btnDelete_Click(object sender, EventArgs e)
        {
            int id = Convert.ToInt32(dataGridView1.CurrentRow.Cells[0].Value);

            SqlCommand cmd = new SqlCommand("sp_DeleteStudent", con);
            cmd.CommandType = CommandType.StoredProcedure;

            cmd.Parameters.AddWithValue("@Id", id);

            con.Open();
            cmd.ExecuteNonQuery();
            con.Close();

            LoadData();
        }

        // LOAD BUTTON
        private void btnLoad_Click(object sender, EventArgs e)
        {
            LoadData();
        }

        // GRID CLICK (optional)
        private void dataGridView1_CellClick(object sender, DataGridViewCellEventArgs e)
        {
            txtName.Text = dataGridView1.CurrentRow.Cells[1].Value.ToString();
            txtCity.Text = dataGridView1.CurrentRow.Cells[2].Value.ToString();
        }
    }
}
